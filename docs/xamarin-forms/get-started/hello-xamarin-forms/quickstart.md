---
title: Xamarin.Forms-Schnellstart
description: In diesem Artikel wird erläutert, wie Sie eine Anwendung erstellen, die eine vom Benutzer eingegebene alphanumerische Telefonnummer in eine numerische Telefonnummer übersetzt und sie anschließend anruft.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 3f2f9c2d-d204-43bc-8c8a-a55ce1e6d2c8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/13/2018
ms.openlocfilehash: f836d6212dfdff6c9840e2d780def3df72bc7c27
ms.sourcegitcommit: 744c0a50420bb091fca8b92a84c20e61c741cf9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/01/2018
ms.locfileid: "52742884"
---
# <a name="xamarinforms-quickstart"></a>Xamarin.Forms-Schnellstart

In dieser exemplarischen Vorgehensweise wird erläutert, wie Sie eine Anwendung erstellen, die eine vom Benutzer eingegebene alphanumerische Telefonnummer in eine numerische Telefonnummer übersetzt und sie anschließend anruft. Die fertige Anwendung wird unten gezeigt:

[![](quickstart-images/intro-app-examples-sml.png "Phoneword-Anwendung")](quickstart-images/intro-app-examples.png#lightbox "Phoneword Application")

::: zone pivot="windows"

## <a name="get-started-with-visual-studio"></a>Erste Schritte mit Visual Studio

1. Starten Sie Visual Studio auf dem **Startbildschirm**. Dadurch wird die Startseite geöffnet:

    ![](quickstart-images/vs/start-page.png "Visual Studio")

2. Klicken Sie in Visual Studio auf **Neues Projekt erstellen…**, um ein neues Projekt zu erstellen:

    ![](quickstart-images/vs/new-solution.png "Neues Projekt")

3. Klicken Sie im Dialogfeld **Neues Projekt** auf **Plattformübergreifend**, wählen Sie die Vorlage **Mobile App (Xamarin.Forms)** aus, geben Sie für „Name“ **Phoneword** ein, wählen Sie einen geeigneten Speicherort für das Projekt aus, und klicken Sie auf **OK**:

    ![](quickstart-images/vs/new-project.w157.png "Plattformübergreifende Projektvorlagen")

    > [!NOTE]
    > Für die C#- und XAML-Codeausschnitte in diesem Schnellstart ist es erforderlich, dass die Projektmappe **Phoneword** genannt wird.
    > Die Verwendung eines anderen Projektmappennamens führt zu zahlreichen Buildfehlern, wenn Sie Code aus diesen Anweisungen in die Projekte kopieren.

4. Klicken Sie im Dialogfeld **New Cross Platform App** (Neue plattformübergreifende App) auf **Leere App**, wählen Sie **.NET Standard** als Strategie für die Codefreigabe aus, und klicken Sie auf **OK**:

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
                <On Platform="Android, UWP" Value="20" />
            </OnPlatform>
        </ContentPage.Padding>
        <StackLayout>
          <Label Text="Enter a Phoneword:" />
          <Entry x:Name="phoneNumberText" Text="1-855-XAMARIN" />
          <Button Text="Translate" Clicked="OnTranslate" />
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
                translatedNumber = PhonewordTranslator.ToNumber (phoneNumberText.Text);
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

9. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **Phoneword**, und klicken Sie auf **Hinzufügen > Neues Element…**:

    ![](quickstart-images/vs/add-new-item.png "Neues Element hinzufügen")

10. Klicken Sie im Dialogfeld **Neues Element hinzufügen** auf **Visual C# > Code > Klasse**, nennen Sie die neue Datei **PhoneTranslator**, und klicken Sie auf **Hinzufügen**:

    ![](quickstart-images/vs/add-translator-class.w157.png "Neue Klasse hinzufügen")

11. Entfernen Sie in **PhoneTranslator.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden. Mit diesem Code wird eine Buchstabenwahl in eine Rufnummer übersetzt:

    ```csharp
    using System.Text;

    namespace Phoneword
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

12. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **Phoneword**, und klicken Sie auf **Hinzufügen > Neues Element…**:

    ![](quickstart-images/vs/add-new-item.png "Neues Element hinzufügen")

13. Klicken Sie im Dialogfeld **Neues Element hinzufügen** auf **Visual C# > Code > Schnittstelle**, nennen Sie die neue Datei **IDialer**, und klicken Sie auf **Hinzufügen**:

    ![](quickstart-images/vs/add-idialer-interface.w157.png "Neue Schnittstelle hinzufügen")

14. Entfernen Sie in **IDialer.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden. Dieser Code definiert eine `Dial`-Methode, die auf jeder Plattform implementiert werden muss, um eine übersetzte Telefonnummer zu wählen:

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

15. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **Phoneword.iOS**, und klicken Sie auf **Hinzufügen > Neues Element…**:

    ![](quickstart-images/vs/add-new-item-ios.png "Neues Element hinzufügen")

16. Klicken Sie im Dialogfeld **Neues Element hinzufügen** auf **Apple > Code > Klasse**, nennen Sie die neue Datei **PhoneDialer**, und klicken Sie auf **Hinzufügen**:

    ![](quickstart-images/vs/new-phone-dialer-ios.w157.png "Neue Klasse hinzufügen")

17. Entfernen Sie in **PhoneDialer.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden. Dieser Code erstellt die Methode <code>Dial</code>, die auf der iOS-Plattform für die Anwahl übersetzter Telefonnummern verwendet wird:

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

18. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **Phoneword.Android**, und klicken Sie auf **Hinzufügen > Neues Element…**:

    ![](quickstart-images/vs/add-new-item-android.png "Neues Element hinzufügen")

19. Klicken Sie im Dialogfeld **Neues Element hinzufügen** auf **Visual C# > Android > Klasse**, nennen Sie die neue Datei **PhoneDialer**, und klicken Sie auf **Hinzufügen**:

    ![](quickstart-images/vs/new-phone-dialer-android.w157.png "Neue Klasse hinzufügen")

20. Entfernen Sie in **PhoneDialer.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden. Dieser Code erstellt die Methode `Dial`, die auf der Android-Plattform für die Anwahl übersetzter Telefonnummern verwendet wird:

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

                var intent = new Intent (Intent.ActionDial);
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

    Beachten Sie, dass dieser Code voraussetzt, dass Sie die neueste Android-API verwenden. Speichern Sie die Änderungen an **PhoneDialer.cs**, indem Sie **STRG+S** drücken, und schließen Sie die Datei.

21. Doppelklicken Sie im **Projektmappen-Explorer** im Projekt **Phoneword.Android** auf **MainActivity.cs**, um sie zu öffnen. Entfernen Sie anschließend den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden Code:

    ```csharp
    using Android.App;
    using Android.Content.PM;
    using Android.OS;

    namespace Phoneword.Droid
    {
        [Activity(Label = "Phoneword", Icon = "@mipmap/icon", Theme = "@style/MainTheme", MainLauncher = true,
                  ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
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

22. Doppelklicken Sie im **Projektmappen-Explorer** im Projekt **Phoneword.Android** auf **Eigenschaften**, und wählen Sie die Registerkarte **Android-Manifest** aus:

    ![](quickstart-images/vs/android-manifest.png "Android-Manifestdatei öffnen")

23. Aktivieren Sie im Abschnitt **Erforderliche Berechtigungen** die Berechtigung **CALL_PHONE**. So erhält die Anwendung die Berechtigung, einen Telefonanruf zu tätigen:

    ![](quickstart-images/vs/android-manifest-changed.png "CallPhone-Berechtigung aktivieren")

    Speichern Sie die Änderungen am Manifest, indem Sie **STRG+S** drücken, und schließen Sie die Datei.

24. Klicken Sie mit der rechten Maustaste auf das Android-Anwendungsprojekt, und wählen Sie **Als Startprojekt festlegen** aus.

25. Starten Sie die Android-App über den „grünen Pfeil“ (Symbolleistenschaltfläche), oder wählen Sie im Menü **Debuggen > Debuggen starten** aus.

    > [!WARNING]
    > Anrufe werden in keinem Simulator unterstützt, daher funktioniert dieses Feature möglicherweise nicht.

26. Wenn Sie ein iOS-Gerät haben und die Systemanforderungen für Mac für die Xamarin.Forms-Entwicklung erfüllt werden, verwenden Sie ein ähnliches Verfahren, um die App für das iOS-Gerät bereitzustellen. Alternativ können Sie die App dem [iOS-Remotesimulator](~/tools/ios-simulator/index.md) bereitstellen.

::: zone-end
::: zone pivot="macos"

## <a name="get-started-with-visual-studio-for-mac"></a>Erste Schritte mit Visual Studio für Mac

1. Starten Sie Visual Studio für Mac, und klicken Sie auf der Startseite auf **Neues Projekt...**, um ein neues Projekt zu erstellen:

    ![](quickstart-images/xs/new-solution.png "Neue Projektmappe")

2. Klicken Sie im Dialogfeld **Vorlage für neues Projekt auswählen** auf **Multi-Plattform > App**, wählen Sie die Vorlage **Leere Forms-App** aus, und klicken Sie auf **Weiter**:

    ![](quickstart-images/xs/choose-template.png "Wählen einer Vorlage")

3. Benennen Sie im Dialogfeld **Configure your Blank Forms app** (Ihre leere Forms-App konfigurieren) die neue App **Phoneword**, stellen Sie sicher, dass das Optionsfeld **Use .NET Standard** (.NET Standard verwenden) ausgewählt ist, und klicken Sie auf **Weiter**:

    ![](quickstart-images/xs/configure-app.png "Forms-Anwendung konfigurieren")

4. Belassen Sie im Dialogfeld **Configure your new Blank Forms app** (Ihre neue leere Forms-App konfigurieren) die Namen von Projektmappen und Projektdateien bei **Phoneword**, wählen Sie einen geeigneten Speicherort für das Projekt aus, und klicken Sie auf **Erstellen**:

    ![](quickstart-images/xs/configure-project.png "Forms-Projekt konfigurieren")

    > [!NOTE]
    > Für die C#- und XAML-Codeausschnitte in diesem Schnellstart ist es erforderlich, dass die Projektmappe **Phoneword** genannt wird.
    > Die Verwendung eines anderen Projektmappennamens führt zu zahlreichen Buildfehlern, wenn Sie Code aus diesen Anweisungen in die Projekte kopieren.

5. Doppelklicken Sie im **Lösungspad** auf **MainPage.xaml**, um es zu öffnen:

    ![](quickstart-images/xs/open-mainpage-xaml.png "„MainPage.xaml“ öffnen")

6. Entfernen Sie in **MainPage.xaml** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden. Dieser Code definiert die Benutzeroberfläche für die Seite deklarativ:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       x:Class="Phoneword.MainPage">
        <ContentPage.Padding>
            <OnPlatform x:TypeArguments="Thickness">
                <On Platform="iOS" Value="20, 40, 20, 20" />
                <On Platform="Android, UWP" Value="20" />
            </OnPlatform>
        </ContentPage.Padding>
        <StackLayout>
          <Label Text="Enter a Phoneword:" />
          <Entry x:Name="phoneNumberText" Text="1-855-XAMARIN" />
          <Button Text="Translate" Clicked="OnTranslate" />
          <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
        </StackLayout>
    </ContentPage>
    ```

    Speichern Sie die Änderungen an **MainPage.xaml**, indem Sie auf **Datei > Speichern** klicken (oder indem Sie **&#8984;+S** drücken), und schließen Sie die Datei.

7. Doppelklicken Sie im **Lösungspad** auf **MainPage.xaml.cs**, um es zu öffnen:

    ![](quickstart-images/xs/open-mainpage-codebehind.png "„MainPage.xaml.cs“ öffnen")

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
                translatedNumber = PhonewordTranslator.ToNumber (phoneNumberText.Text);
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

9. Wählen Sie im **Lösungspad** das Projekt **Phoneword** aus, klicken Sie mit der rechten Maustaste darauf, und klicken Sie auf **Hinzufügen > Neue Datei…**:

    ![](quickstart-images/xs/add-new-translator-file.png "Neue Datei hinzufügen")

10. Klicken Sie im Dialogfeld **Neue Datei** auf **Allgemein > Leere Klasse**, nennen Sie die neue Datei **PhoneTranslator**, und klicken Sie auf **Neu**:

    ![](quickstart-images/xs/add-translator-class.png "Neue Klasse hinzufügen")

11. Entfernen Sie in **PhoneTranslator.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden. Mit diesem Code wird eine Buchstabenwahl in eine Rufnummer übersetzt:

    ```csharp
    using System.Text;

    namespace Phoneword
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

12. Wählen Sie im **Lösungspad** das Projekt **Phoneword** aus, klicken Sie mit der rechten Maustaste darauf, und klicken Sie auf **Hinzufügen > Neue Datei…**:

    ![](quickstart-images/xs/add-new-interface.png "Neue Datei hinzufügen")

13. Klicken Sie im Dialogfeld **Neue Datei** auf **Allgemein > Leere Schnittstelle**, nennen Sie die neue Datei **IDialer**, und klicken Sie auf **Neu**:

    ![](quickstart-images/xs/add-idialer-interface.png "Neue Schnittstelle hinzufügen")

14. Entfernen Sie in **IDialer.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden. Dieser Code definiert eine `Dial`-Methode, die auf jeder Plattform implementiert werden muss, um eine übersetzte Telefonnummer zu wählen:

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

15. Wählen Sie im **Lösungspad** das Projekt **Phoneword.iOS** aus, klicken Sie mit der rechten Maustaste, und wählen Sie **Hinzufügen > Neue Datei…** aus:

    ![](quickstart-images/xs/add-new-file-ios.png "Neue Datei hinzufügen")

16. Klicken Sie im Dialogfeld **Neue Datei** auf **Allgemein > Leere Klasse**, nennen Sie die neue Datei **PhoneDialer**, und klicken Sie auf **Neu**:

    ![](quickstart-images/xs/new-phonedialer-ios.png "Neue Klasse hinzufügen")

17. Entfernen Sie in **PhoneDialer.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden. Dieser Code erstellt die Methode `Dial`, die auf der iOS-Plattform für die Anwahl übersetzter Telefonnummern verwendet wird:

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

18. Wählen Sie im **Lösungspad** das Projekt **Phoneword.Droid** aus, klicken Sie mit der rechten Maustaste darauf und dann auf **Hinzufügen > Neue Datei…**:

    ![](quickstart-images/xs/add-new-file-android.png "Neue Datei hinzufügen")

19. Klicken Sie im Dialogfeld **Neue Datei** auf **Allgemein > Leere Klasse**, nennen Sie die neue Datei **PhoneDialer**, und klicken Sie auf **Neu**:

    ![](quickstart-images/xs/new-phonedialer-android.png "Neue Klasse hinzufügen")

20. Entfernen Sie in **PhoneDialer.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden. Dieser Code erstellt die Methode `Dial`, die auf der Android-Plattform für die Anwahl übersetzter Telefonnummern verwendet wird:

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

                var intent = new Intent (Intent.ActionDial);
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

    Beachten Sie, dass dieser Code voraussetzt, dass Sie die neueste Android-API verwenden. Speichern Sie die Änderungen an **PhoneDialer.cs**, indem Sie auf **Datei > Speichern** klicken (oder indem Sie **&#8984;+S** drücken), und schließen Sie die Datei.

21. Doppelklicken Sie im **Lösungspad** im Projekt **Phoneword.Droid** auf **MainActivity.cs**, um sie zu öffnen. Entfernen Sie anschließend den gesamten Vorlagencode, und ersetzen Sie ihn durch folgenden Code:

    ```csharp
    using Android.App;
    using Android.Content.PM;
    using Android.OS;

    namespace Phoneword.Droid
    {
        [Activity(Label = "Phoneword", Icon = "@mipmap/icon", Theme = "@style/MainTheme", MainLauncher = true,
                  ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
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

22. Erweitern Sie im **Lösungspad** den Ordner **Eigenschaften**, und doppelklicken Sie auf die Datei **AndroidManifest.xml**:

    ![](quickstart-images/xs/android-manifest.png "Android-Manifestdatei öffnen")

23. Aktivieren Sie im Abschnitt **Erforderliche Berechtigungen** die Berechtigung **CallPhone**. So erhält die Anwendung die Berechtigung, einen Telefonanruf zu tätigen:

    ![](quickstart-images/xs/android-manifest-changed.png "CallPhone-Berechtigung aktivieren")

    Speichern Sie die Änderungen an **AndroidManifest.xml**, indem Sie auf **Datei > Speichern** klicken (oder indem Sie **&#8984;+S** drücken), und schließen Sie die Datei.

24. Wählen Sie in Visual Studio für Mac die Menüelemente **Erstellen > Alle erstellen** aus, oder drücken Sie **&#8984;+B**. Die Anwendung wird erstellt, und in der Symbolleiste von Visual Studio für Mac wird eine Erfolgsmeldung angezeigt.

    ![](quickstart-images/xs/build-successful.png "Buildvorgang erfolgreich")

25. Wenn Fehler auftreten, wiederholen Sie die vorherigen Schritte, und beheben Sie sie, bis die Anwendung erfolgreich erstellt wird.
26. Klicken Sie in der Symbolleiste von Visual Studio für Mac auf **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im iOS-Simulator zu starten:

    ![](quickstart-images/xs/start.png "Symbolleiste für Visual Studio für Mac")
    ![](quickstart-images/xs/phoneword-result-ios.png "iOS-Simulator")

    Hinweis: Anrufe werden im iOS-Simulator nicht unterstützt.

27. Wählen Sie im **Lösungspad** das Projekt **Phoneword.Droid** aus, klicken Sie mit der rechten Maustaste darauf, und wählen Sie **Als Startprojekt festlegen** aus:

    ![](quickstart-images/xs/set-startup-project.png "Als Startprojekt festlegen")

28. Klicken Sie in der Symbolleiste von Visual Studio für Mac auf **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung in einem Android-Emulator zu starten:

    ![](quickstart-images/xs/phoneword-result-android.png "Android-Emulator")

    > [!WARNING]
    > Anrufe werden in Android-Emulatoren nicht unterstützt.

::: zone-end

Herzlichen Glückwunsch zum Fertigstellen einer Xamarin.Forms-Anwendung. Im [nächsten Abschnitt](~/xamarin-forms/get-started/hello-xamarin-forms/deepdive.md) in diesem Handbuch werden die Schritte aus dieser exemplarischen Vorgehensweise wiederholt, um ein Verständnis für die Grundlagen der Anwendungsentwicklung mit Xamarin.Forms zu erarbeiten.

## <a name="related-links"></a>Verwandte Links

- [Accessing Native Features via the DependencyService (Zugriff auf native Funktionen über DependencyService)](~/xamarin-forms/app-fundamentals/dependency-service/index.md)
- [Phoneword (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/Phoneword/)
