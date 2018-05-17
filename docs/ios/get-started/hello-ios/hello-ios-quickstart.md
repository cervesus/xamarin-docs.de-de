---
title: Hallo, iOS-Schnellstart
description: Dieser zweiteilige Leitfaden beschreibt, wie Sie mit Visual Studio für Mac oder Visual Studio eine einfache Xamarin.iOS-Anwendung erstellen. Außerdem erhalten Sie Einblick in die grundlegenden Aspekte der Entwicklung von iOS-Anwendungen mit Xamarin. Es werden die Tools, Konzepte und Schritte eingeführt, die zum Erstellen und Bereitstellen einer Xamarin.iOS-Anwendung erforderlich sind.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: D3868F3A-4EED-BDDF-45AA-665102C39634
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/02/2017
ms.openlocfilehash: c82343b3ec36512a8cfd7ba3b96862eac14bfafd
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="helloios-quickstart"></a>Schnellstart für Hallo.iOS

Dieses Handbuch beschreibt, wie Sie eine Anwendung erstellen, die eine vom Benutzer eingegebene alphanumerische Telefonnummer in eine numerische Telefonnummer übersetzt und anschließend diese Nummer anruft. Die endgültige Anwendung sieht wie folgt aus:

 [![](hello-ios-quickstart-images/image1.png "Die Schnellstart-App „Hello.iOS“")](hello-ios-quickstart-images/image1.png#lightbox)

<a name="Requirements" />

## <a name="requirements"></a>Anforderungen

Die iOS-Entwicklung mit Xamarin erfordert:

-  Ein Mac mit macOS Sierra (10.12) oder höher
-  Die Installation der neuesten Version von Xcode und iOS SDK aus dem [App Store](https://itunes.apple.com/us/app/xcode/id497799835?mt=12).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Xamarin.iOS funktioniert mit den folgenden Setups:

-  Neueste Version von Visual Studio für Mac, die den oben aufgeführten Spezifikationen entspricht.

Im [Xamarin.iOS Mac Installation guide (Xamarin.iOS Mac-Installationsleitfaden)](~/ios/get-started/installation/mac.md) finden Sie ausführliche Installationsanweisungen.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Xamarin.iOS funktioniert mit den folgenden Setups:

-  Neueste Version von Visual Studio 2017 Community, Professional oder Enterprise unter Windows 7 oder höher, gekoppelt mit einem Mac-Buildhost, der den oben aufgeführten Spezifikationen entspricht.

Im [Xamarin.iOS Windows Installation guide (Xamarin.iOS Windows-Installationshandbuch)](~/ios/get-started/installation/windows/index.md) finden Sie ausführliche Installationsanweisungen.

-----

Bevor Sie starten, laden Sie den [Xamarin App-Symbolsatz](https://github.com/xamarin/ios-samples/blob/master/Hello_iOS/Resources/XamarinAppIconsandLaunchImages.zip?raw=true) herunter.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

## <a name="visual-studio-for-mac-walkthrough"></a>Exemplarische Vorgehensweise für Visual Studio für Mac

In dieser exemplarischen Vorgehensweise wird erläutert, wie Sie eine Anwendung namens Phoneword erstellen, die eine alphanumerische Telefonnummer in eine numerische Telefonnummer übersetzt.

1. Starten Sie Visual Studio für Mac aus dem Ordner **Anwendungen** oder aus **Spotlight**, um den Startbildschirm zu öffnen:

  ![](hello-ios-quickstart-images/image2new.png "Der Startbildschirm")

Klicken Sie auf dem Startbildschirm auf **Neues Projekt...**, um eine neue Xamarin.iOS-Projektmappe zu erstellen:

![](hello-ios-quickstart-images/image3new.png "iOS-Lösung")

2. Im **Dialogfeld „Neue Projektmappe“**, wählen Sie die Vorlage **iOS > App > Single View Application** (iOS > App > Einzelansicht) aus, um sicherzustellen, dass C# ausgewählt ist. Klicken Sie auf **Weiter**:

  ![](hello-ios-quickstart-images/image4new.png "Einzelansicht-App wählen")

3. Konfigurieren Sie die App. Geben Sie ihr den **Namen** `Phoneword_iOS`, und lassen Sie alles andere in der Standardeinstellung. Klicken Sie auf **Weiter**:

  ![](hello-ios-quickstart-images/image5new.png "Geben Sie den Namen der App ein")

4. Behalten Sie den Projekt- und den Projektmappennamen bei. Wählen Sie hier den Speicherort des Projekts, oder behalten sie ihn standardmäßig:

  ![](hello-ios-quickstart-images/image6new.png "Wählen Sie den Speicherort des Projekts")

5. Klicken Sie auf **Erstellen**, um die **Projektmappe** zu erstellen.

6. Öffnen Sie die **Main.storyboard**-Datei durch Doppelklicken auf diese im **Projektmappenpad**. Dies bietet Ihnen die Möglichkeit, eine Benutzeroberfläche visuell zu erstellen:

  ![](hello-ios-quickstart-images/image7new.png "Der iOS-Designer")

  Beachten Sie, dass die _Größenklassen_ standardmäßig aktiviert sind. Weitere Informationen zu diesen Größenklassen finden Sie im [Unified Storyboards](~/ios/user-interface/storyboards/unified-storyboards.md)-Handbuch.

8. Geben Sie im **Pad „Toolbox“** „Bezeichnung“ in die Suchleiste ein, und ziehen Sie eine **Bezeichnung** auf die Entwurfsoberfläche (der Bereich in der Mitte):

  ![](hello-ios-quickstart-images/image8new.png "Ziehen Sie eine Bezeichnung auf die Entwurfsoberfläche, den Bereich in der Mitte")

  > [!NOTE]
  > Sie können das **Eigenschaftenpad** oder die **Toolbox** jederzeit durch Navigieren zu **Ansicht > Bereiche** öffnen.

9. Ziehen Sie die Ziehpunkte der *Dragging Controls* (Ziehsteuerelemente) (die Kreise, um das Steuerelement herum), und verbreitern Sie die Bezeichnung:

  ![](hello-ios-quickstart-images/image9.png "Verbreitern Sie die Bezeichnung")


10. Nachdem Sie die **Bezeichnung** auf der Entwurfsoberfläche ausgewählt haben, verwenden Sie das **Eigenschaftenpad**, um die **Text**-Eigenschaft von der **Bezeichnung** in „Enter a Phoneword:“ (Phoneword eingeben) zu ändern.

  ![](hello-ios-quickstart-images/image10.png "Legen Sie die Bezeichnung auf „Phoneword eingeben“ fest")

11. Suchen Sie nach „Textfeld“ in der Toolbox, und ziehen Sie ein **Textfeld** aus der **Toolbox** auf die Entwurfsoberfläche, und platzieren Sie es unter der **Bezeichnung**. Passen Sie die Breite so an, bis die Breite des **Textfelds** mit jener der **Bezeichnung** identisch ist:

  ![](hello-ios-quickstart-images/image12new.png "Legen Sie für das Textfeld und die Bezeichnung die gleiche Breite fest")


12. Nachdem Sie das **Textfeld** auf der Entwurfsoberfläche ausgewählt haben, ändern Sie die **Name**-Eigenschaft des **Textfelds** im Identitätsabschnitt des **Eigenschaftenpads** in `PhoneNumberText`, und ändern Sie die **Text**-Eigenschaft in „1-855-XAMARIN“:

  ![](hello-ios-quickstart-images/image13new.png "Ändern Sie die Eigenschaft „Titel“ in 1-855-XAMARIN")


13. Ziehen Sie aus der **Toolbox** eine **Schaltfläche** auf die Entwurfsoberfläche, und platzieren Sie diese unterhalb des **Textfelds**: Passen Sie die Breite so an, bis die Breite der **Schaltfläche** mit dem **Textfeld** und der **Bezeichnung** identisch ist:

  ![](hello-ios-quickstart-images/image14new.png "Passen Sie die Breite so an, dass die Schaltfläche, das Textfeld und die Bezeichnung gleich breit sind")


14. Nachdem Sie die **Schaltfläche** auf der Entwurfsoberfläche ausgewählt haben, ändern Sie die **Name**-Eigenschaft im **Identitäts**-Teil des **Eigenschaftenpad** in `TranslateButton`. Ändern Sie die **Titel**-Eigenschaft in „Übersetzen“:

  ![](hello-ios-quickstart-images/image15new.png "Ändern Sie die Eigenschaft „Titel“ in „Übersetzen“")


15. Wiederholen Sie die beiden obigen Schritte, und ziehen Sie eine **Schaltfläche** aus der **Toolbox** auf die Entwurfsoberfläche, und platzieren Sie diese unterhalb der ersten **Schaltfläche**. Passen Sie die Breite solange an, bis die Breite der **Schaltfläche** jener der ersten **Schaltfläche** entspricht:

  ![](hello-ios-quickstart-images/image16new.png "Passen Sie die Breite so an, dass die Schaltfläche genauso breit wie die erste Schaltfläche ist")


16. Nachdem Sie die zweite **Schaltfläche** auf der Entwurfsoberfläche ausgewählt haben, ändern Sie die **Name**-Eigenschaft im **Identitäts**-Teil des **Eigenschaftenpad** in `CallButton`. Ändern Sie die **Titel**-Eigenschaft in „Aufruf“:

  ![](hello-ios-quickstart-images/image17new.png "Ändern Sie die Eigenschaft „Titel“ in „Anrufen“")

  Speichern Sie die Änderungen durch Navigieren zu **Datei > Speichern** oder durch Drücken von **⌘+S**.


17. Der App muss für die Übersetzung von Telefonnummern von alphanumerisch in numerisch eine Logik hinzugefügt werden. Fügen Sie dem Projekt eine neue Datei hinzu, indem Sie mit der rechten Maustaste auf das **Phoneword_iOS**-Projekt im **Projektmappenpad** klicken und **Hinzufügen > Neue Datei...** auswählen oder **⌘+N** drücken:

  ![](hello-ios-quickstart-images/image18.png "Fügen Sie dem Projekt eine neue Datei hinzu")


18. Wählen Sie im **Neue Datei**-Dialogfeld **Allgemein > Leere Klasse** aus, und geben Sie der neuen Datei den Namen `PhoneTranslator`:

  ![](hello-ios-quickstart-images/image19.png "Wählen Sie „Leere Klasse“, und geben Sie der neuen Datei den Namen „PhoneTranslator“")


19. Dadurch wird eine neue, leere C#-Klasse für uns erstellt. Entfernen Sie den gesamten Vorlagencode, und ersetzen Sie Ihn durch den Folgenden:

    ```csharp
    using System.Text;
    using System;

    namespace Phoneword_iOS
    {
        public static class PhoneTranslator
        {
            public static string ToNumber(string raw)
            {
                if (string.IsNullOrWhiteSpace(raw)) {
                    return "";
                } else {
                    raw = raw.ToUpperInvariant();
                }

                var newNumber = new StringBuilder();
                foreach (var c in raw)
                {
                    if (" -0123456789".Contains(c)) {
                        newNumber.Append(c);
                    } else {
                        var result = TranslateToNumber(c);
                        if (result != null) {
                            newNumber.Append(result);
                        }
                    }
                    // otherwise we've skipped a non-numeric char
                }
                return newNumber.ToString();
            }

            static bool Contains (this string keyString, char c)
            {
                return keyString.IndexOf(c) >= 0;
            }

            static int? TranslateToNumber(char c)
            {
                if ("ABC".Contains(c)) {
                    return 2;
                } else if ("DEF".Contains(c)) {
                    return 3;
                } else if ("GHI".Contains(c)) {
                    return 4;
                } else if ("JKL".Contains(c)) {
                    return 5;
                } else if ("MNO".Contains(c)) {
                    return 6;
                } else if ("PQRS".Contains(c)) {
                    return 7;
                } else if ("TUV".Contains(c)) {
                    return 8;
                } else if ("WXYZ".Contains(c)) {
                    return 9;
                }
                return null;
            }
        }
    }
    ```

    Speichern Sie die **PhoneTranslator.cs**-Datei, und schließen Sie sie.

20. Fügen Sie Code hinzu, um die Benutzeroberfläche zu verknüpfen. Doppelklicken Sie dazu auf **ViewController.cs** im **Projektmappenpad**, um ihn zu öffnen:

  ![](hello-ios-quickstart-images/image20new.png "Fügen Sie Code hinzu, um die Benutzeroberfläche zu verknüpfen")


21. Verknüpfen Sie zunächst die `TranslateButton`. Wählen Sie in der **Ansichtskontrolle**-Klasse die `ViewDidLoad`-Methode aus, und fügen Sie den folgenden Code unter dem `base.ViewDidLoad()`-Aufruf hinzu:

    ```csharp
    string translatedNumber = "";

    TranslateButton.TouchUpInside += (object sender, EventArgs e) => {
        // Convert the phone number with text to a number
        // using PhoneTranslator.cs
        translatedNumber = PhoneTranslator.ToNumber(
            PhoneNumberText.Text);

        // Dismiss the keyboard if text field was tapped
        PhoneNumberText.ResignFirstResponder ();

        if (translatedNumber == "") {
            CallButton.SetTitle ("Call ", UIControlState.Normal);
            CallButton.Enabled = false;
        } else {
            CallButton.SetTitle ("Call " + translatedNumber,
                UIControlState.Normal);
            CallButton.Enabled = true;
        }
    };
    ```

    Schließen Sie `using Phoneword_iOS;` mit ein, wenn der Namespace der Datei unterschiedlich ist.

22. Fügen Sie Code hinzu, um auf den Benutzer zu reagieren, indem Sie auf die zweite Schaltfläche drücken, die den Namen `CallButton` besitzt. Fügen Sie den folgenden Code unter dem Code für die `TranslateButton` ein, und fügen Sie am Anfang der Datei `using Foundation;` hinzu:

    ```csharp
        CallButton.TouchUpInside += (object sender, EventArgs e) => {
            // Use URL handler with tel: prefix to invoke Apple's Phone app...
            var url = new NSUrl ("tel:" + translatedNumber);

            // ...otherwise show an alert dialog
            if (!UIApplication.SharedApplication.OpenUrl (url)) {
                var alert = UIAlertController.Create ("Not supported", "Scheme 'tel:' is not supported on this device", UIAlertControllerStyle.Alert);
                alert.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
                PresentViewController (alert, true, null);
            }
        };
    ```


23. Speichern Sie die Änderungen, und erstellen Sie anschließend die Anwendung, indem Sie **Erstellen > Alle erstellen** auswählen oder **⌘+B** drücken.  Wenn die Anwendung kompiliert wird, wird eine Erfolgsmeldung oberhalb der IDE angezeigt:

  ![](hello-ios-quickstart-images/image21.png "Eine Erfolgsmeldung wird oberhalb der IDE angezeigt")

  Wenn Fehler auftreten, wiederholen Sie die vorherigen Schritte, und beheben Sie alle Fehler, bis die Anwendung erfolgreich erstellt wird.

27. Testen Sie schließlich die Anwendung im **iOS-Simulator**. Wählen Sie oben links in der IDE **Debuggen** aus der ersten Dropdownliste und **iPhone 8 Plus iOS x.x** aus der zweiten Dropdownliste aus, und drücken Sie **Starten** (die dreieckige Schaltfläche, die einer Wiedergabeschaltfläche ähnelt):

  ![](hello-ios-quickstart-images/image27new.png "Klicken Sie auf „Start“")

  > [!NOTE]
  > Aktuell kann es aufgrund einer Anforderung von Apple erforderlich sein, ein Entwicklungszertifikat oder eine *Signierungsidentität* zu besitzen, um Code für ein Gerät oder den Simulator zu erstellen. Führen Sie dazu die Schritte im Handbuch [Gerätebereitstellung](~/ios/get-started/installation/device-provisioning/manual-provisioning.md) aus.

28. Damit wird die Anwendung im iOS-Simulator gestartet.

  ![](hello-ios-quickstart-images/image28.png "Die im iOS-Simulator ausgeführte Anwendung")

  Telefonanrufe werden im iOS-Simulator nicht unterstützt. Stattdessen wird beim Versuch, einen Anruf zu tätigen, ein Warnhinweis angezeigt:

  ![](hello-ios-quickstart-images/image29.png "Das Dialogfeld „Warnung“ beim Versuch, einen Anruf zu tätigen")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="visual-studio-walkthrough"></a>Visual Studio: Exemplarische Vorgehensweise

In dieser exemplarischen Vorgehensweise wird erläutert, wie Sie eine Anwendung namens Phoneword erstellen, die eine alphanumerische Telefonnummer in eine numerische Telefonnummer übersetzt.

**Hinweis**: In dieser exemplarischen Vorgehensweise wird Visual Studio Enterprise 2017 auf einem virtuellen Computer mit Windows 10 verwendet. Ihre Einrichtung kann sich davon unterscheiden, solange sie die oben genannten Anforderungen erfüllt, aber beachten Sie, dass einige Screenshots sich von Ihrer Einrichtung unterscheiden können.

> [!NOTE]
> Bevor Sie mit dieser exemplarischen Vorgehensweise fortfahren, müssen Sie bereits eine Verbindung von Visual Studio zu Ihrem Mac hergestellt haben. Dies liegt daran, dass Xamarin.iOS auf die Apple-Tools zum Erstellen und Starten des iOS Designers und der Anwendungen zurückgreift. Führen Sie zur Einrichtung die Schritte im Leitfaden [Durchführen einer Kopplung mit einem Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md) aus.

1. Starten Sie Visual Studio im **Start**-Menü:

  ![](hello-ios-quickstart-images/image001-.png "Der Startbildschirm")

  Erstellen Sie eine neue Xamarin.iOS-Projektmappe, indem Sie **Datei > Neu > Projekt... > Visual C# > iPhone & iPad > iOS-App (Xamari)** auswählen:

  ![Wählen Sie den iOS-App (Xamarin)-Projekttyp aus](hello-ios-quickstart-images/image002.w157.png "Wählen Sie den iOS-App (Xamarin)-Projekttyp aus")

  Wählen Sie im nächsten Dialogfeld die Vorlage **Einzelansicht-App** aus, und drücken Sie **OK**, um das Projekt zu erstellen:

  ![Wählen Sie die Einzelansicht-Projektvorlage aus](hello-ios-quickstart-images/image002-2.w157.png "Wählen Sie die Einzelansicht-Projektvorlage aus")

1. Vergewissern Sie sich, dass das Xamarin Mac Agent-Symbol auf der Symbolleiste grün ist.

    ![Vergewissern Sie sich, dass das Xamarin Mac Agent-Symbol auf der Symbolleiste grün ist.](hello-ios-quickstart-images/vs-image4.png)

    Wenn dies nicht der Fall ist, besteht keine Verbindung zu Ihrem Mac-Buildhost. Befolgen Sie dann die Schritte im [Konfigurationshandbuch](~/ios/get-started/installation/windows/connecting-to-mac/index.md), um eine Verbindung herzustellen.

1. Öffnen Sie die **Main.storyboard**-Datei im iOS Designer durch Doppelklicken auf diese im **Projektmappen-Explorer**:

  ![](hello-ios-quickstart-images/vs-image7.png "Der iOS-Designer")

1. Öffnen Sie die **Toolbox**-Registerkarte, geben Sie „Bezeichnung“ in die Suchleiste ein, und ziehen Sie eine **Bezeichnung** auf die Entwurfsoberfläche (der Bereich in der Mitte):

  ![](hello-ios-quickstart-images/vs-image8.png "Ziehen Sie eine Bezeichnung auf die Entwurfsoberfläche, den Bereich in der Mitte")

1. Ziehen Sie dann die Ziehpunkte der *Dragging Controls* (Ziehsteuerelemente), und verbreitern Sie die Bezeichnung:

  ![](hello-ios-quickstart-images/vs-image9.png "Verbreitern Sie die Bezeichnung")

1. Nachdem Sie die **Bezeichnung** auf der Entwurfsoberfläche ausgewählt haben, verwenden Sie das **Eigenschaftenfenster**, um die **Text**-Eigenschaft von der **Bezeichnung** in „Enter a Phoneword:“ (Phoneword eingeben) zu ändern.

  ![](hello-ios-quickstart-images/vs-image10.png "Ändern Sie die Eigenschaft „Text“ der Bezeichnung in „Phoneword eingeben“")

  > [!NOTE]
  > Sie können die **Eigenschaften** oder die **Toolbox** jederzeit durch Navigieren zum Menü **Ansicht** nutzen.

1. Suchen Sie nach „Textfeld“ in der Toolbox, und ziehen Sie ein **Textfeld** aus der **Toolbox** auf die Entwurfsoberfläche, und platzieren Sie es unter der **Bezeichnung**. Passen Sie die Breite so an, bis die Breite des **Textfelds** mit jener der **Bezeichnung** identisch ist:

  ![](hello-ios-quickstart-images/vs-image12.png "Passen Sie die Breite so an, dass Textfeld und Bezeichnung gleich breit sind")

10. Nachdem Sie das **Textfeld** auf der Entwurfsoberfläche ausgewählt haben, ändern Sie die **Name**-Eigenschaft des **Textfelds** im Identitätsabschnitt der **Eigenschaften** in `PhoneNumberText`, und ändern Sie die **Text**-Eigenschaft in „1-855-XAMARIN“:

  ![](hello-ios-quickstart-images/vs-image13.png "Ändern Sie die Eigenschaft „Text“ in „1-855-XAMARIN“")


11. Ziehen Sie aus der **Toolbox** eine **Schaltfläche** auf die Entwurfsoberfläche, und platzieren Sie diese unterhalb des **Textfelds**: Passen Sie die Breite so an, bis die Breite der **Schaltfläche** mit dem **Textfeld** und der **Bezeichnung** identisch ist:

  ![](hello-ios-quickstart-images/vs-image14.png "Passen Sie die Breite so an, dass die Schaltfläche, das Textfeld und die Bezeichnung gleich breit sind")


12. Nachdem Sie die **Schaltfläche** auf der Entwurfsoberfläche ausgewählt haben, ändern Sie die **Name**-Eigenschaft im **Identitäts**-Teil der **Eigenschaften** in `TranslateButton`. Ändern Sie die **Titel**-Eigenschaft in „Übersetzen“:

  ![](hello-ios-quickstart-images/vs-image15.png "Ändern Sie die Eigenschaft „Titel“ in „Übersetzen“")


13. Wiederholen Sie die beiden obigen Schritte, und ziehen Sie eine **Schaltfläche** aus der **Toolbox** auf die Entwurfsoberfläche, und platzieren Sie diese unterhalb der ersten **Schaltfläche**. Passen Sie die Breite solange an, bis die Breite der **Schaltfläche** jener der ersten **Schaltfläche** entspricht:

  ![](hello-ios-quickstart-images/vs-image16.png "Passen Sie die Breite so an, dass die Schaltfläche genauso breit wie die erste Schaltfläche ist")


14. Nachdem Sie die zweite **Schaltfläche** auf der Entwurfsoberfläche ausgewählt haben, ändern Sie die **Name**-Eigenschaft im **Identitäts**-Teil der **Eigenschaften** in `CallButton`. Ändern Sie die **Titel**-Eigenschaft in „Aufruf“:

  ![](hello-ios-quickstart-images/vs-image17.png "Ändern Sie die Eigenschaft „Titel“ in „Anrufen“")

  Speichern Sie die Änderungen durch Navigieren zu **Datei > Alle speichern** oder durch Drücken von **STRG+S**.

15. Fügen Sie Code hinzu, um alphanumerische Telefonnummern in numerische Telefonnummern zu übersetzen. Dazu fügen Sie dem Projekt eine neue Datei hinzu, indem Sie mit der rechten Maustaste auf das **Phoneword**-Projekt im **Projektmappen-Explorer** klicken und **Hinzufügen > Neue Datei...** auswählen oder **Ctrl + Shift + A** drücken:

  ![](hello-ios-quickstart-images/vs-image18.png "Fügen Sie Code hinzu, um alphanumerische Telefonnummern in numerische Telefonnummern zu übersetzen")


16. Wählen Sie im Dialogfeld **Neues Element hinzufügen** (mit der rechten Maustaste auf das Projekt klicken und „Hinzufügen > Neues Element...“ auswählen) die Option **Apple > Klasse** aus, und nennen Sie die neue Datei `PhoneTranslator`:

  ![](hello-ios-quickstart-images/vs-image19.w157.png "Fügen Sie eine neue Klasse mit dem Namen „PhoneTranslator“ hinzu")

  > [!IMPORTANT]
  > Stellen Sie sicher, dass Sie die Vorlage „Klasse“ auswählen, die ein C# im Symbol enthält. Andernfalls können Sie nicht auf diese neue Klasse verweisen.


17. Dadurch wird eine neue C#-Klasse erstellt. Entfernen Sie den gesamten Vorlagencode, und ersetzen Sie Ihn durch den Folgenden:

    ```csharp
    using System.Text;
    using System;

    namespace Phoneword
    {
        public static class PhoneTranslator
        {
            public static string ToNumber(string raw)
            {
                if (string.IsNullOrWhiteSpace(raw)) {
                    return "";
                } else {
                    raw = raw.ToUpperInvariant();
                }

                var newNumber = new StringBuilder();
                foreach (var c in raw)
                {
                    if (" -0123456789".Contains(c)) {
                        newNumber.Append(c);
                    } else {
                        var result = TranslateToNumber(c);
                        if (result != null) {
                            newNumber.Append(result);
                        }
                    }
                    // otherwise we've skipped a non-numeric char
                }
                return newNumber.ToString();
            }

            static bool Contains (this string keyString, char c)
            {
                return keyString.IndexOf(c) >= 0;
            }

            static int? TranslateToNumber(char c)
            {
                if ("ABC".Contains(c)) {
                    return 2;
                } else if ("DEF".Contains(c)) {
                    return 3;
                } else if ("GHI".Contains(c)) {
                    return 4;
                } else if ("JKL".Contains(c)) {
                    return 5;
                } else if ("MNO".Contains(c)) {
                    return 6;
                } else if ("PQRS".Contains(c)) {
                    return 7;
                } else if ("TUV".Contains(c)) {
                    return 8;
                } else if ("WXYZ".Contains(c)) {
                    return 9;
                }
                return null;
            }
        }
    }
    ```

  Speichern Sie die **PhoneTranslator.cs**-Datei, und schließen Sie sie.

18. Doppelklicken Sie auf **ViewController.cs** im **Projektmappen-Explorer**, um ihn zu öffnen, damit den Handles-Interaktionen mit den Schaltflächen eine Logik hinzugefügt werden kann:

  ![](hello-ios-quickstart-images/vs-image20.png "Logik zum Behandeln von Interaktionen mit den Schaltflächen hinzugefügt")


19. Verknüpfen Sie zunächst die `TranslateButton`. Wählen Sie in der **ViewController**-Klasse die `ViewDidLoad`-Methode aus. Fügen Sie den folgenden Schaltflächencode in `ViewDidLoad` unter dem `base.ViewDidLoad()`-Aufruf hinzu:

    ```csharp
    string translatedNumber = "";

    TranslateButton.TouchUpInside += (object sender, EventArgs e) => {

        // Convert the phone number with text to a number
        // using PhoneTranslator.cs
        translatedNumber = PhoneTranslator.ToNumber(PhoneNumberText.Text);

        // Dismiss the keyboard if text field was tapped
        PhoneNumberText.ResignFirstResponder ();

        if (translatedNumber == "") {
            CallButton.SetTitle ("Call", UIControlState.Normal);
            CallButton.Enabled = false;
            }
        else {
            CallButton.SetTitle ("Call " + translatedNumber, UIControlState.Normal);
            CallButton.Enabled = true;
            }
    };
    ```
    Schließen Sie `using Phoneword;` mit ein, wenn der Namespace der Datei unterschiedlich ist.

20. Fügen Sie Code hinzu, um auf den Benutzer zu reagieren, indem Sie auf die zweite Schaltfläche drücken, die den Namen `CallButton` besitzt. Fügen Sie den folgenden Code unter dem Code für die `TranslateButton` ein, und fügen Sie am Anfang der Datei `using Foundation;` hinzu:

    ```csharp
    CallButton.TouchUpInside += (object sender, EventArgs e) => {
        var url = new NSUrl ("tel:" + translatedNumber);

            // Use URL handler with tel: prefix to invoke Apple's Phone app,
            // otherwise show an alert dialog

        if (!UIApplication.SharedApplication.OpenUrl (url)) {
                        var alert = UIAlertController.Create ("Not supported", "Scheme 'tel:' is not supported on this device", UIAlertControllerStyle.Alert);
                        alert.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
                        PresentViewController (alert, true, null);
                    }
    };
    ```


21. Speichern Sie die Änderungen, und erstellen Sie anschließend die Anwendung, indem Sie **Erstellen > Projektmappe erstellen** auswählen oder **STRG+Shift+B** drücken.  Wenn die Anwendung kompiliert wird, wird eine Erfolgsmeldung unterhalb der IDE angezeigt:

  ![](hello-ios-quickstart-images/vs-image21.png "Eine Erfolgsmeldung wird unterhalb der IDE angezeigt")

  Wenn Fehler auftreten, wiederholen Sie die vorherigen Schritte, und beheben Sie alle Fehler, bis die Anwendung erfolgreich erstellt wird.

22. Testen Sie schließlich die Anwendung im **remoten iOS-Simulator**. Klicken Sie im Dropdownmenü in der IDE-Symbolleiste auf **Debuggen** und **iPhone 8 iOS x.x**, und drücken Sie **Starten** (das grüne Dreieck, das einer Wiedergabe-Schaltfläche ähnelt):

  ![](hello-ios-quickstart-images/vs-image27.png "Klicken Sie auf „Start“")

23. Damit wird die Anwendung im iOS-Simulator gestartet.

  ![](hello-ios-quickstart-images/vs-image28.png "Die im iOS-Simulator ausgeführte Anwendung")

  Telefonanrufe werden im iOS-Simulator nicht unterstützt. Stattdessen wird beim Versuch, einen Anruf zu tätigen, ein Warnhinweis angezeigt:

  ![](hello-ios-quickstart-images/vs-image29.png "Das Dialogfeld „Warnung“ beim Versuch, einen Anruf zu tätigen")

-----

Herzlichen Glückwunsch, Sie haben Ihre erste Xamarin.iOS-Anwendung fertiggestellt!

Erfahren Sie jetzt mehr über die Tools und Fähigkeiten, die Sie in diesem Handbuch kennengelernt haben, im [Hello, iOS Deep Dive (Ausführliche Erläuterungen zu Hallo, iOS)](~/ios/get-started/hello-ios/hello-ios-deepdive.md).


## <a name="related-links"></a>Verwandte Links

- [Xamarin App-Symbole und Startbilder (Beispiel)](https://github.com/xamarin/ios-samples/blob/master/Hello_iOS/Resources/XamarinAppIconsandLaunchImages.zip?raw=true)
- [Hallo, iOS (Beispiel)](https://developer.xamarin.com/samples/monotouch/Hello_iOS/)
- [iOS-Eingaberichtlinien](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/Introduction/Introduction.html)
- [iOS-Bereitstellungsportal](https://developer.apple.com/ios/manage/overview/index.action)
