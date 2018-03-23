---
title: Xamarin.Forms-Schnellstart
ms.topic: article
ms.prod: xamarin
ms.assetid: 3f2f9c2d-d204-43bc-8c8a-a55ce1e6d2c8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/06/2018
ms.openlocfilehash: 5ee36d63e2751eb5d09ee526755d62dda4ef537e
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2018
---
# <a name="xamarinforms-quickstart"></a>Xamarin.Forms-Schnellstart

In dieser exemplarischen Vorgehensweise wird erläutert, wie Sie eine Anwendung erstellen, die eine vom Benutzer eingegebene alphanumerische Telefonnummer in eine numerische Telefonnummer übersetzt und sie anschließend anruft. Die fertige Anwendung wird unten gezeigt:

[![](quickstart-images/intro-app-examples-sml.png "Phoneword-Anwendung")](quickstart-images/intro-app-examples.png#lightbox "Phoneword Application")

Erstellen Sie die Phoneword-Anwendung wie folgt:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Starten Sie Visual Studio auf dem **Startbildschirm**. Dadurch wird die Startseite geöffnet:

    ![](quickstart-images/vs/start-page.png "Visual Studio")

2. Klicken Sie in Visual Studio auf **Neues Projekt erstellen…**, um ein neues Projekt zu erstellen:

    ![](quickstart-images/vs/new-solution.png "Neues Projekt")

3. Klicken Sie im Dialogfeld **Neues Projekt** auf **Plattformübergreifend**, wählen Sie die Vorlage **Plattformübergreifende App (Xamarin.Forms)** aus, geben Sie für „Name“ und „Projektmappenname“ `Phoneword` ein, wählen Sie einen geeigneten Speicherort für das Projekt aus, und klicken Sie auf **OK**:

    ![](quickstart-images/vs/new-project.png "Plattformübergreifende Projektvorlagen")

4. Klicken Sie im Dialogfeld **New Cross Platform App** (Neue plattformübergreifende App) auf **Leere App**, wählen Sie **Xamarin.Forms** als UI-Technologie und **.NET Standard** als Strategie für die Codefreigabe aus, und klicken Sie auf **OK**:

    ![](quickstart-images/vs/new-app.png "Neue plattformübergreifende App")

5. Doppelklicken Sie im **Projektmappen-Explorer** im Projekt **Phoneword** auf die Datei **MainPage.xaml**, um sie zu öffnen:

    ![](quickstart-images/vs/open-mainpage-xaml.png "„MainPage.xaml“ öffnen")

6. Entfernen Sie in **MainPage.xaml** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden. Dieser Code definiert die Benutzeroberfläche für die Seite deklarativ:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       x:Class="Phoneword.MainPage">
        <ContentPage.Padding>
            <OnPlatform x:TypeArguments="Thickness">
                <On Platform="iOS" Value="20, 40, 20, 20" />
                <On Platform="Android, WinPhone, Windows" Value="20" />
            </OnPlatform>
        </ContentPage.Padding>
        <StackLayout>
          <Label Text="Enter a Phoneword:" />
          <Entry x:Name="phoneNumberText" Text="1-855-XAMARIN" />
          <Button x:Name="translateButon" Text="Translate" Clicked="OnTranslate" />
          <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
        </StackLayout>
    </ContentPage>
    ```

    Speichern Sie die Änderungen an **MainPage.xaml**, indem Sie **STRG+S** drücken, und schließen Sie die Datei.

7. Erweitern Sie **MainPage.xaml** im **Projektmappen-Explorer**, und doppelklicken Sie dann auf die Datei **MainPage.xaml.cs**, um sie zu öffnen:

    ![](quickstart-images/vs/open-mainpage-codebehind.png "„MainPage.xaml.cs“ öffnen")

8. Entfernen Sie in **MainPage.xaml.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden. Die Methoden `OnTranslate` und `OnCall` werden ausgeführt, wenn in der Benutzeroberfläche auf **Translate** (Übersetzen) und **Aufrufen** geklickt wird:

    ```csharp
    using System;
    using Xamarin.Forms;

    namespace Phoneword
    {
        public partial class MainPage : ContentPage
        {
            string translatedNumber;

            public MainPage ()
            {
                InitializeComponent ();
            }

            void OnTranslate (object sender, EventArgs e)
            {
                translatedNumber = Core.PhonewordTranslator.ToNumber (phoneNumberText.Text);
                if (!string.IsNullOrWhiteSpace (translatedNumber)) {
                    callButton.IsEnabled = true;
                    callButton.Text = "Call " + translatedNumber;
                } else {
                    callButton.IsEnabled = false;
                    callButton.Text = "Call";
                }
            }

            async void OnCall (object sender, EventArgs e)
            {
                if (await this.DisplayAlert (
                        "Dial a Number",
                        "Would you like to call " + translatedNumber + "?",
                        "Yes",
                        "No")) {
                    var dialer = DependencyService.Get<IDialer> ();
                    if (dialer != null)
                        dialer.Dial (translatedNumber);
                }
            }
        }
    }
    ```

    > [!NOTE]
    > Der Versuch, die Anwendung an diesem Punkt zu erstellen, führt zu Fehlern, die später behoben werden.

    Speichern Sie die Änderungen an **MainPage.xaml.cs**, indem Sie **STRG+S** drücken, und schließen Sie die Datei.

9. Erweitern Sie **App.xaml** im **Projektmappen-Explorer**, und doppelklicken Sie dann auf die Datei **App.xaml.cs**, um sie zu öffnen:

    ![](quickstart-images/vs/open-app-class.png "„App.Xaml.cs“ öffnen")

10. Entfernen Sie in **App.xaml.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden. Der `App`-Konstruktor legt die `MainPage`-Klasse als die Seite fest, die beim Starten der Anwendung angezeigt wird:

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Xaml;

    [assembly: XamlCompilation(XamlCompilationOptions.Compile)]
    namespace Phoneword
    {
        public partial class App : Application
        {
            public App()
            {
                InitializeComponent();
                MainPage = new MainPage();
            }

            protected override void OnStart()
            {
                // Handle when your app starts
            }

            protected override void OnSleep()
            {
                // Handle when your app sleeps
            }

            protected override void OnResume()
            {
                // Handle when your app resumes
            }
        }
    }
    ```

    Speichern Sie die Änderungen an **App.xaml.cs**, indem Sie **STRG+S** drücken, und schließen Sie die Datei.

11. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **Phoneword**, und klicken Sie auf **Hinzufügen > Neues Element…**:

    ![](quickstart-images/vs/add-new-item.png "Neues Element hinzufügen")

12. Klicken Sie im Dialogfeld **Neues Element hinzufügen** auf **Visual C# > Code > Klasse**, nennen Sie die neue Datei **PhoneTranslator**, und klicken Sie auf **Hinzufügen**:

    ![](quickstart-images/vs/add-translator-class.png "Neue Klasse hinzufügen")

13. Entfernen Sie in **PhoneTranslator.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden. Mit diesem Code wird eine Buchstabenwahl in eine Rufnummer übersetzt:

    ```csharp
    using System.Text;

    namespace Core
    {
        public static class PhonewordTranslator
        {
            public static string ToNumber(string raw)
            {
                if (string.IsNullOrWhiteSpace(raw))
                    return null;

                raw = raw.ToUpperInvariant();

                var newNumber = new StringBuilder();
                foreach (var c in raw)
                {
                    if (" -0123456789".Contains(c))
                        newNumber.Append(c);
                    else
                    {
                        var result = TranslateToNumber(c);
                        if (result != null)
                            newNumber.Append(result);
                        // Bad character?
                        else
                            return null;
                    }
                }
                return newNumber.ToString();
            }

            static bool Contains(this string keyString, char c)
            {
                return keyString.IndexOf(c) >= 0;
            }

            static readonly string[] digits = {
                "ABC", "DEF", "GHI", "JKL", "MNO", "PQRS", "TUV", "WXYZ"
            };

            static int? TranslateToNumber(char c)
            {
                for (int i = 0; i < digits.Length; i++)
                {
                    if (digits[i].Contains(c))
                        return 2 + i;
                }
                return null;
            }
        }
    }
    ```

    Speichern Sie die Änderungen an **PhoneTranslator.cs**, indem Sie **STRG+S** drücken, und schließen Sie die Datei.

14. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **Phoneword**, und klicken Sie auf **Hinzufügen > Neues Element…**:

    ![](quickstart-images/vs/add-new-item.png "Neues Element hinzufügen")

15. Klicken Sie im Dialogfeld **Neues Element hinzufügen** auf **Visual C# > Code > Schnittstelle**, nennen Sie die neue Datei **IDialer**, und klicken Sie auf **Hinzufügen**:

    ![](quickstart-images/vs/add-idialer-interface.png "Neue Schnittstelle hinzufügen")

16. Entfernen Sie in **IDialer.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden. Dieser Code definiert eine `Dial`-Methode, die auf jeder Plattform implementiert werden muss, um eine übersetzte Telefonnummer zu wählen:

    ```csharp
    namespace Phoneword
    {
        public interface IDialer
        {
            bool Dial(string number);
        }
    }
    ```

    Speichern Sie die Änderungen an **IDialer.cs**, indem Sie **STRG+S** drücken, und schließen Sie die Datei.

    > [!NOTE]
    > Der allgemeine Code für die Anwendung ist damit fertiggestellt. Nun wird plattformspezifischer Telefonwählcode als [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md) implementiert.

17. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **Phoneword.iOS**, und klicken Sie auf **Hinzufügen > Neues Element…**:

    ![](quickstart-images/vs/add-new-item-ios.png "Neues Element hinzufügen")

18. Klicken Sie im Dialogfeld **Neues Element hinzufügen** auf **Apple > Code > Klasse**, nennen Sie die neue Datei **PhoneDialer**, und klicken Sie auf **Hinzufügen**:

    ![](quickstart-images/vs/new-phone-dialer-ios.png "Neue Klasse hinzufügen")

19. Entfernen Sie in **PhoneDialer.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden. Dieser Code erstellt die Methode <code>Dial</code>, die auf der iOS-Plattform für die Anwahl übersetzter Telefonnummern verwendet wird:

    ```csharp
    using Foundation;
    using Phoneword.iOS;
    using UIKit;
    using Xamarin.Forms;

    [assembly: Dependency(typeof(PhoneDialer))]
    namespace Phoneword.iOS
    {
        public class PhoneDialer : IDialer
        {
            public bool Dial(string number)
            {
                return UIApplication.SharedApplication.OpenUrl (
                    new NSUrl ("tel:" + number));
            }
        }
    }
    ```

    Speichern Sie die Änderungen an **PhoneDialer.cs**, indem Sie **STRG+S** drücken, und schließen Sie die Datei.

20. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **Phoneword.Android**, und klicken Sie auf **Hinzufügen > Neues Element…**:

    ![](quickstart-images/vs/add-new-item-android.png "Neues Element hinzufügen")

21. Klicken Sie im Dialogfeld **Neues Element hinzufügen** auf **Visual C# > Android > Klasse**, nennen Sie die neue Datei **PhoneDialer**, und klicken Sie auf **Hinzufügen**:

    ![](quickstart-images/vs/new-phone-dialer-android.png "Neue Klasse hinzufügen")

22. Entfernen Sie in **PhoneDialer.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden. Dieser Code erstellt die Methode `Dial`, die auf der Android-Plattform für die Anwahl übersetzter Telefonnummern verwendet wird:

    ```csharp
    using Android.Content;
    using Android.Telephony;
    using Phoneword.Droid;
    using System.Linq;
    using Xamarin.Forms;
    using Uri = Android.Net.Uri;

    [assembly: Dependency(typeof(PhoneDialer))]
    namespace Phoneword.Droid
    {
        public class PhoneDialer : IDialer
        {
            public bool Dial(string number)
            {
                var context = MainActivity.Instance;
                if (context == null)
                    return false;

                var intent = new Intent (Intent.ActionCall);
                intent.SetData (Uri.Parse ("tel:" + number));

                if (IsIntentAvailable (context, intent)) {
                    context.StartActivity (intent);
                    return true;
                }

                return false;
            }

            public static bool IsIntentAvailable(Context context, Intent intent)
            {
                var packageManager = context.PackageManager;

                var list = packageManager.QueryIntentServices (intent, 0)
                    .Union (packageManager.QueryIntentActivities (intent, 0));

                if (list.Any ())
                    return true;

                var manager = TelephonyManager.FromContext (context);
                return manager.PhoneType != PhoneType.None;
            }
        }
    }
    ```

    Speichern Sie die Änderungen an **PhoneDialer.cs**, indem Sie **STRG+S** drücken, und schließen Sie die Datei.

23. Doppelklicken Sie im **Projektmappen-Explorer** im Projekt **Phoneword.Android** auf **MainActivity.cs**, um sie zu öffnen. Entfernen Sie anschließend den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden Code:

    ```csharp
    using Android.App;
    using Android.Content.PM;
    using Android.OS;

    namespace Phoneword.Droid
    {
        [Activity(Label = "Phoneword", Icon = "@drawable/icon", Theme = "@style/MainTheme", MainLauncher = true, ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
        public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
        {
            internal static MainActivity Instance { get; private set; }

            protected override void OnCreate(Bundle bundle)
            {
                TabLayoutResource = Resource.Layout.Tabbar;
                ToolbarResource = Resource.Layout.Toolbar;

                base.OnCreate(bundle);

                Instance = this;
                global::Xamarin.Forms.Forms.Init(this, bundle);
                LoadApplication(new App());
            }
        }
    }  
    ```

    Speichern Sie die Änderungen an **MainActivity.cs**, indem Sie **STRG+S** drücken, und schließen Sie die Datei.

24. Doppelklicken Sie im **Projektmappen-Explorer** im Projekt **Phoneword.Android** auf **Eigenschaften**, und wählen Sie die Registerkarte **Android-Manifest** aus:

    ![](quickstart-images/vs/android-manifest.png "Android-Manifestdatei öffnen")

25. Aktivieren Sie im Abschnitt **Erforderliche Berechtigungen** die Berechtigung **CALL_PHONE**. So erhält die Anwendung die Berechtigung, einen Telefonanruf zu tätigen:

    ![](quickstart-images/vs/android-manifest-changed.png "CallPhone-Berechtigung aktivieren")

    Speichern Sie die Änderungen am Manifest, indem Sie **STRG+S** drücken, und schließen Sie die Datei.

26. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **Phoneword.UWP**, und klicken Sie auf **Hinzufügen > Neues Element…**:

    ![](quickstart-images/vs/add-new-item-uwp.png "Neues Element hinzufügen")

27. Klicken Sie im Dialogfeld **Neues Element hinzufügen** auf **Visual C# > Code > Klasse**, nennen Sie die neue Datei **PhoneDialer**, und klicken Sie auf **Hinzufügen**:

    ![](quickstart-images/vs/new-phone-dialer-uwp.png "Neue Klasse hinzufügen")

28. Entfernen Sie in **PhoneDialer.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden. Dieser Code erstellt die Methode `Dial` und Hilfsmethoden, die auf der universellen Windows-Plattform für die Anwahl übersetzter Telefonnummern verwendet werden:

    ```csharp
    using Phoneword.UWP;
    using System;
    using System.Threading.Tasks;
    using Windows.ApplicationModel.Calls;
    using Windows.UI.Popups;
    using Xamarin.Forms;

    [assembly: Dependency(typeof(PhoneDialer))]
    namespace Phoneword.UWP
    {
        public class PhoneDialer : IDialer
        {
            bool dialled = false;

            public bool Dial(string number)
            {
                DialNumber(number);
                return dialled;
            }

            async Task DialNumber(string number)
            {
                var phoneLine = await GetDefaultPhoneLineAsync();
                if (phoneLine != null)
                {
                    phoneLine.Dial(number, number);
                    dialled = true;
                }
                else
                {
                    var dialog = new MessageDialog("No line found to place the call");
                    await dialog.ShowAsync();
                    dialled = false;
                }
            }

            async Task<PhoneLine> GetDefaultPhoneLineAsync()
            {
                var phoneCallStore = await PhoneCallManager.RequestStoreAsync();
                var lineId = await phoneCallStore.GetDefaultLineAsync();
                return await PhoneLine.FromIdAsync(lineId);
            }
        }
    }
    ```

    Speichern Sie die Änderungen an **PhoneDialer.cs**, indem Sie **STRG+S** drücken, und schließen Sie die Datei.

29. Klicken Sie im **Projektmappen-Explorer** im Projekt **Phoneword.UWP** mit der rechten Maustaste auf **Verweise**, und wählen Sie anschließend **Verweis hinzufügen…** aus:

    ![](quickstart-images/vs/uwp-add-reference.png "Verweis hinzufügen")

30. Klicken Sie im Dialogfeld **Verweis-Manager** auf **Universelles Windows > Erweiterungen > Windows Mobile Extensions for UWP** (Mobile Windows-Erweiterungen für die UWP), und klicken Sie auf **OK**:

    ![](quickstart-images/vs/uwp-add-reference-extensions.png "Mobile Windows-Erweiterungen für UWP hinzufügen")

31. Doppelklicken Sie im **Projektmappen-Explorer** im Projekt **Phoneword.UWP** auf **Package.appxmanifest**:

    ![](quickstart-images/vs/uwp-manifest.png "UWP-Manifest öffnen")

31. Aktivieren Sie auf der Seite **Funktionen** die Funktion **Telefonanruf**. So erhält die Anwendung die Berechtigung, einen Telefonanruf zu tätigen:

    ![](quickstart-images/vs/uwp-manifest-changed.png "Anruffunktion aktivieren")

    Speichern Sie die Änderungen am Manifest, indem Sie **STRG+S** drücken, und schließen Sie die Datei.

32. Klicken Sie in Visual Studio auf die Menüelemente **Erstellen > Projektmappe erstellen**, oder drücken Sie **STRG+UMSCHALT+B**. Die Anwendung wird erstellt, und eine Erfolgsmeldung wird in der Statusleiste von Visual Studio angezeigt:

    ![](quickstart-images/vs/build-successful.png "Buildvorgang erfolgreich")

    Wenn Fehler auftreten, wiederholen Sie die vorherigen Schritte, und beheben Sie sie, bis die Anwendung erfolgreich erstellt wird.

33. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **Phoneword.UWP**, und klicken Sie auf **Als Startprojekt festlegen**:

    ![](quickstart-images/vs/uwp-set-as-startup-project.png "Als Startprojekt festlegen")

34. Klicken Sie in der Symbolleiste von Visual Studio auf **Starten** (die dreieckige Schaltfläche, die der Play-Taste ähnelt), um die Anwendung zu starten:

    ![](quickstart-images/vs/start.png "Visual Studio-Symbolleiste")
    ![](quickstart-images/vs/phone-result-uwp.png "Phoneword-Anwendung für UWP")

35. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **Phoneword.Android**, und klicken Sie auf **Als Startprojekt festlegen**.
36. Klicken Sie in der Symbolleiste von Visual Studio auf **Starten** (die dreieckige Wiedergabetaste), um die Anwendung in einem Android-Emulator zu starten.
37. Wenn Sie ein iOS-Gerät haben und die Systemanforderungen für Mac für die Xamarin.Forms-Entwicklung erfüllt werden, verwenden Sie ein ähnliches Verfahren, um die App für das iOS-Gerät bereitzustellen. Alternativ können Sie die App dem [iOS-Remotesimulator](~/tools/ios-simulator.md) bereitstellen.

    Hinweis: Anrufe werden in keinem Simulator unterstützt.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Starten Sie Visual Studio für Mac, und klicken Sie auf der Startseite auf **Neues Projekt...**, um ein neues Projekt zu erstellen:

    ![](quickstart-images/xs/new-solution.png "Neue Projektmappe")

2. Klicken Sie im Dialogfeld **Vorlage für neues Projekt auswählen** auf **Multi-Plattform > App**, wählen Sie die Vorlage **Leere Forms-App** aus, und klicken Sie auf **Weiter**:

    ![](quickstart-images/xs/choose-template.png "Wählen einer Vorlage")

3. Geben Sie der neuen App im Dialogfeld **Configure your new Blank Forms app** (Ihre neue leere Forms-App konfigurieren) den Namen `Phoneword`. Stellen Sie anschließend sicher, dass das Optionsfeld **Portable Klassenbibliothek verwenden** ausgewählt und das Kästchen **XAML für Benutzerschnittstellendateien verwenden** aktiviert ist, und klicken Sie auf **Weiter**:

    ![](quickstart-images/xs/configure-app.png "Forms-Anwendung konfigurieren")

4. Belassen Sie im Dialogfeld **Ihre neue leere Forms-App konfigurieren** die Namen von Projektmappen und Projektdateien bei `Phoneword`, wählen Sie einen geeigneten Speicherort für das Projekt aus, und klicken Sie auf **Erstellen**:

    ![](quickstart-images/xs/configure-project.png "Forms-Projekt konfigurieren")

5. Wählen Sie im **Lösungspad** das Projekt **Phoneword** aus, klicken Sie mit der rechten Maustaste darauf, und klicken Sie auf **Hinzufügen > Neue Datei…**:

    ![](quickstart-images/xs/add-new-file.png "Neue Datei hinzufügen")

6. Klicken Sie im Dialogfeld **Neue Datei** auf **Formular > XAML für Formular-ContentPage**, nennen Sie die neue Datei **MainPage**, und klicken Sie auf **Neu**. Dadurch wird dem Projekt eine Seite namens **MainPage** hinzugefügt:

    ![](quickstart-images/xs/add-mainpage-class.png "Neue ContentPage-Seite hinzufügen")

7. Doppelklicken Sie im **Lösungspad** auf **MainPage.xaml**, um es zu öffnen:

    ![](quickstart-images/xs/open-mainpage-xaml.png "„MainPage.xaml“ öffnen")

8. Entfernen Sie in **MainPage.xaml** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden. Dieser Code definiert die Benutzeroberfläche für die Seite deklarativ:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       x:Class="Phoneword.MainPage">
        <ContentPage.Padding>
            <OnPlatform x:TypeArguments="Thickness">
                <On Platform="iOS" Value="20, 40, 20, 20" />
                <On Platform="Android, WinPhone, Windows" Value="20" />
            </OnPlatform>
        </ContentPage.Padding>
        <StackLayout>
          <Label Text="Enter a Phoneword:" />
          <Entry x:Name="phoneNumberText" Text="1-855-XAMARIN" />
          <Button x:Name="translateButon" Text="Translate" Clicked="OnTranslate" />
          <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
        </StackLayout>
    </ContentPage>
    ```

    Speichern Sie die Änderungen an **MainPage.xaml**, indem Sie auf **Datei > Speichern** klicken (oder indem Sie **&#8984;+S** drücken), und schließen Sie die Datei.

9. Doppelklicken Sie im **Lösungspad** auf **MainPage.xaml.cs**, um es zu öffnen:

    ![](quickstart-images/xs/open-mainpage-codebehind.png "„MainPage.xaml.cs“ öffnen")

10. Entfernen Sie in **MainPage.xaml.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden. Die Methoden `OnTranslate` und `OnCall` werden ausgeführt, wenn in der Benutzeroberfläche auf **Translate** (Übersetzen) und **Aufrufen** geklickt wird:

    ```csharp
    using System;
    using Xamarin.Forms;

    namespace Phoneword
    {
        public partial class MainPage : ContentPage
        {
            string translatedNumber;

            public MainPage ()
            {
                InitializeComponent ();
            }

            void OnTranslate (object sender, EventArgs e)
            {
                translatedNumber = Core.PhonewordTranslator.ToNumber (phoneNumberText.Text);
                if (!string.IsNullOrWhiteSpace (translatedNumber)) {
                    callButton.IsEnabled = true;
                    callButton.Text = "Call " + translatedNumber;
                } else {
                    callButton.IsEnabled = false;
                    callButton.Text = "Call";
                }
            }

            async void OnCall (object sender, EventArgs e)
            {
                if (await this.DisplayAlert (
                        "Dial a Number",
                        "Would you like to call " + translatedNumber + "?",
                        "Yes",
                        "No")) {
                    var dialer = DependencyService.Get<IDialer> ();
                    if (dialer != null)
                        dialer.Dial (translatedNumber);
                }
            }
        }
    }
    ```

    > [!NOTE]
    > Der Versuch, die Anwendung an diesem Punkt zu erstellen, führt zu Fehlern, die später behoben werden.

    Speichern Sie die Änderungen an **MainPage.xaml.cs**, indem Sie auf **Datei > Speichern** klicken (oder indem Sie **&#8984;+S** drücken), und schließen Sie die Datei.

11. Doppelklicken Sie im **Lösungspad** auf **App.xaml.cs**, um es zu öffnen:

    ![](quickstart-images/xs/open-app-class.png "„App.Xaml.cs“ öffnen")

12. Entfernen Sie in **App.xaml.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden. Der `App`-Konstruktor legt die `MainPage`-Klasse als die Seite fest, die beim Starten der Anwendung angezeigt wird:

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Xaml;

    [assembly: XamlCompilation(XamlCompilationOptions.Compile)]
    namespace Phoneword
    {
        public partial class App : Application
        {
            public App()
            {
                InitializeComponent();
                MainPage = new MainPage();
            }

            protected override void OnStart()
            {
                // Handle when your app starts
            }

            protected override void OnSleep()
            {
                // Handle when your app sleeps
            }

            protected override void OnResume()
            {
                // Handle when your app resumes
            }
        }
    }
    ```

    Speichern Sie die Änderungen an **Phoneword.cs**, indem Sie auf **Datei > Speichern** klicken (oder indem Sie **&#8984;+S** drücken), und schließen Sie die Datei.

13. Wählen Sie im **Lösungspad** das Projekt **Phoneword** aus, klicken Sie mit der rechten Maustaste darauf, und klicken Sie auf **Hinzufügen > Neue Datei…**:

    ![](quickstart-images/xs/add-new-translator-file.png "Neue Datei hinzufügen")

14. Klicken Sie im Dialogfeld **Neue Datei** auf **Allgemein > Leere Klasse**, nennen Sie die neue Datei **PhoneTranslator**, und klicken Sie auf **Neu**:

    ![](quickstart-images/xs/add-translator-class.png "Neue Klasse hinzufügen")

15. Entfernen Sie in **PhoneTranslator.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden. Mit diesem Code wird eine Buchstabenwahl in eine Rufnummer übersetzt:

    ```csharp
    using System.Text;

    namespace Core
    {
        public static class PhonewordTranslator
        {
            public static string ToNumber(string raw)
            {
                if (string.IsNullOrWhiteSpace(raw))
                    return null;

                raw = raw.ToUpperInvariant();

                var newNumber = new StringBuilder();
                foreach (var c in raw)
                {
                    if (" -0123456789".Contains(c))
                        newNumber.Append(c);
                    else
                    {
                        var result = TranslateToNumber(c);
                        if (result != null)
                            newNumber.Append(result);
                        // Bad character?
                        else
                            return null;
                    }
                }
                return newNumber.ToString();
            }

            static bool Contains(this string keyString, char c)
            {
                return keyString.IndexOf(c) >= 0;
            }

            static readonly string[] digits = {
                "ABC", "DEF", "GHI", "JKL", "MNO", "PQRS", "TUV", "WXYZ"
            };

            static int? TranslateToNumber(char c)
            {
                for (int i = 0; i < digits.Length; i++)
                {
                    if (digits[i].Contains(c))
                        return 2 + i;
                }
                return null;
            }
        }
    }
    ```

    Speichern Sie die Änderungen an **PhoneTranslator.cs**, indem Sie auf **Datei > Speichern** klicken (oder indem Sie **&#8984;+S** drücken), und schließen Sie die Datei.

16. Wählen Sie im **Lösungspad** das Projekt **Phoneword** aus, klicken Sie mit der rechten Maustaste darauf, und klicken Sie auf **Hinzufügen > Neue Datei…**:

    ![](quickstart-images/xs/add-new-interface.png "Neue Datei hinzufügen")

17. Klicken Sie im Dialogfeld **Neue Datei** auf **Allgemein > Leere Schnittstelle**, nennen Sie die neue Datei **IDialer**, und klicken Sie auf **Neu**:

    ![](quickstart-images/xs/add-idialer-interface.png "Neue Schnittstelle hinzufügen")

18. Entfernen Sie in **IDialer.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden. Dieser Code definiert eine `Dial`-Methode, die auf jeder Plattform implementiert werden muss, um eine übersetzte Telefonnummer zu wählen:

    ```csharp
    namespace Phoneword
    {
        public interface IDialer
        {
            bool Dial(string number);
        }
    }
    ```
    Speichern Sie die Änderungen an **IDialer.cs**, indem Sie auf **Datei > Speichern** klicken (oder indem Sie **&#8984;+S** drücken), und schließen Sie die Datei.

    > [!NOTE]
    > Der allgemeine Code für die Anwendung ist damit fertiggestellt. Nun wird plattformspezifischer Telefonwählcode als [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md) implementiert.

19. Wählen Sie im **Lösungspad** das Projekt **Phoneword.iOS** aus, klicken Sie mit der rechten Maustaste, und wählen Sie **Hinzufügen > Neue Datei…** aus:

    ![](quickstart-images/xs/add-new-file-ios.png "Neue Datei hinzufügen")

20. Klicken Sie im Dialogfeld **Neue Datei** auf **Allgemein > Leere Klasse**, nennen Sie die neue Datei **PhoneDialer**, und klicken Sie auf **Neu**:

    ![](quickstart-images/xs/new-phonedialer-ios.png "Neue Klasse hinzufügen")

21. Entfernen Sie in **PhoneDialer.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden. Dieser Code erstellt die Methode `Dial`, die auf der iOS-Plattform für die Anwahl übersetzter Telefonnummern verwendet wird:

    ```csharp
    using Foundation;
    using Phoneword.iOS;
    using UIKit;
    using Xamarin.Forms;

    [assembly: Dependency(typeof(PhoneDialer))]
    namespace Phoneword.iOS
    {
        public class PhoneDialer : IDialer
        {
            public bool Dial(string number)
            {
                return UIApplication.SharedApplication.OpenUrl (
                    new NSUrl ("tel:" + number));
            }
        }
    }
    ```

    Speichern Sie die Änderungen an **PhoneDialer.cs**, indem Sie auf **Datei > Speichern** klicken (oder indem Sie **&#8984;+S** drücken), und schließen Sie die Datei.

22. Wählen Sie im **Lösungspad** das Projekt **Phoneword.Droid** aus, klicken Sie mit der rechten Maustaste darauf und dann auf **Hinzufügen > Neue Datei…**:

    ![](quickstart-images/xs/add-new-file-android.png "Neue Datei hinzufügen")

23. Klicken Sie im Dialogfeld **Neue Datei** auf **Allgemein > Leere Klasse**, nennen Sie die neue Datei **PhoneDialer**, und klicken Sie auf **Neu**:

    ![](quickstart-images/xs/new-phonedialer-android.png "Neue Klasse hinzufügen")

24. Entfernen Sie in **PhoneDialer.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden. Dieser Code erstellt die Methode `Dial`, die auf der Android-Plattform für die Anwahl übersetzter Telefonnummern verwendet wird:

    ```csharp
    using Android.Content;
    using Android.Telephony;
    using Phoneword.Droid;
    using System.Linq;
    using Xamarin.Forms;
    using Uri = Android.Net.Uri;

    [assembly: Dependency(typeof(PhoneDialer))]
    namespace Phoneword.Droid
    {
        public class PhoneDialer : IDialer
        {
            public bool Dial(string number)
            {
                var context = MainActivity.Instance;
                if (context == null)
                    return false;

                var intent = new Intent (Intent.ActionCall);
                intent.SetData (Uri.Parse ("tel:" + number));

                if (IsIntentAvailable (context, intent)) {
                    context.StartActivity (intent);
                    return true;
                }

                return false;
            }

            public static bool IsIntentAvailable(Context context, Intent intent)
            {
                var packageManager = context.PackageManager;

                var list = packageManager.QueryIntentServices (intent, 0)
                    .Union (packageManager.QueryIntentActivities (intent, 0));

                if (list.Any ())
                    return true;

                var manager = TelephonyManager.FromContext (context);
                return manager.PhoneType != PhoneType.None;
            }
        }
    }
    ```

    Speichern Sie die Änderungen an **PhoneDialer.cs**, indem Sie auf **Datei > Speichern** klicken (oder indem Sie **&#8984;+S** drücken), und schließen Sie die Datei.

25. Doppelklicken Sie im **Lösungspad** im Projekt **Phoneword.Droid** auf **MainActivity.cs**, um sie zu öffnen. Entfernen Sie anschließend den gesamten Vorlagencode, und ersetzen Sie ihn durch folgenden Code:

    ```csharp
    using Android.App;
    using Android.Content.PM;
    using Android.OS;

    namespace Phoneword.Droid
    {
        [Activity(Label = "Phoneword", Icon = "@drawable/icon", Theme = "@style/MainTheme", MainLauncher = true, ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
        public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
        {
            internal static MainActivity Instance { get; private set; }

            protected override void OnCreate(Bundle bundle)
            {
                TabLayoutResource = Resource.Layout.Tabbar;
                ToolbarResource = Resource.Layout.Toolbar;

                base.OnCreate(bundle);

                Instance = this;
                global::Xamarin.Forms.Forms.Init(this, bundle);
                LoadApplication(new App());
            }
        }
    }        
    ```

    Speichern Sie die Änderungen an **MainActivity.cs**, indem Sie auf **Datei > Speichern** klicken (oder indem Sie **&#8984;+S** drücken), und schließen Sie die Datei.

26. Erweitern Sie im **Lösungspad** den Ordner **Eigenschaften**, und doppelklicken Sie auf die Datei **AndroidManifest.xml**:

    ![](quickstart-images/xs/android-manifest.png "Android-Manifestdatei öffnen")

27. Aktivieren Sie im Abschnitt **Erforderliche Berechtigungen** die Berechtigung **CallPhone**. So erhält die Anwendung die Berechtigung, einen Telefonanruf zu tätigen:

    ![](quickstart-images/xs/android-manifest-changed.png "CallPhone-Berechtigung aktivieren")

    Speichern Sie die Änderungen an **AndroidManifest.xml**, indem Sie auf **Datei > Speichern** klicken (oder indem Sie **&#8984;+S** drücken), und schließen Sie die Datei.

28. Entfernen Sie im **Lösungspad** die Klasse **PhonewordPage** aus dem Projekt **Phoneword**. Diese Seite wurde automatisch hinzugefügt, als das Projekt erstellt wurde, und ist nicht mehr erforderlich.
29. Wählen Sie in Visual Studio für Mac die Menüelemente **Erstellen > Alle erstellen** aus, oder drücken Sie **&#8984;+B**. Die Anwendung wird erstellt, und in der Symbolleiste von Visual Studio für Mac wird eine Erfolgsmeldung angezeigt.

    ![](quickstart-images/xs/build-successful.png "Buildvorgang erfolgreich")

30. Wenn Fehler auftreten, wiederholen Sie die vorherigen Schritte, und beheben Sie sie, bis die Anwendung erfolgreich erstellt wird.
31. Klicken Sie in der Symbolleiste von Visual Studio für Mac auf **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im iOS-Simulator zu starten:

    ![](quickstart-images/xs/start.png "Symbolleiste für Visual Studio für Mac")
    ![](quickstart-images/xs/phoneword-result-ios.png "iOS-Simulator")

    Hinweis: Anrufe werden im iOS-Simulator nicht unterstützt.

32. Wählen Sie im **Lösungspad** das Projekt **Phoneword.Droid** aus, klicken Sie mit der rechten Maustaste darauf, und wählen Sie **Als Startprojekt festlegen** aus:

    ![](quickstart-images/xs/set-startup-project.png "Als Startprojekt festlegen")

33. Klicken Sie in der Symbolleiste von Visual Studio für Mac auf **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung in einem Android-Emulator zu starten:

    ![](quickstart-images/xs/phoneword-result-android.png "Android-Emulator")

    Hinweis: Anrufe werden im Android-Emulator nicht unterstützt.

-----

Herzlichen Glückwunsch zum Fertigstellen einer Xamarin.Forms-Anwendung. Im [nächsten Abschnitt](~/xamarin-forms/get-started/hello-xamarin-forms/deepdive.md) in diesem Handbuch werden die Schritte aus dieser exemplarischen Vorgehensweise wiederholt, um ein Verständnis für die Grundlagen der Anwendungsentwicklung mit Xamarin.Forms zu erarbeiten.


## <a name="related-links"></a>Verwandte Links

- [Accessing Native Features via the DependencyService (Zugriff auf native Funktionen über DependencyService)](~/xamarin-forms/app-fundamentals/dependency-service/index.md)
- [Phoneword (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/Phoneword/)
