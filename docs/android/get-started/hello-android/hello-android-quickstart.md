---
title: 'Hallo, Android: Schnellstart'
description: In diesem zweiteiligen Handbuch erstellen Sie Ihre erste Xamarin.Android-Anwendung – entweder mit Visual Studio oder mit Visual Studio für Mac. Außerdem entwickeln Sie ein Verständnis für die grundlegenden Aspekte der Entwicklung von Android-Anwendungen mit Xamarin. Währenddessen werden Ihnen die Tools, Konzepte und Schritte vorgestellt, die zum Erstellen und Bereitstellen einer Xamarin.Android-Anwendung erforderlich sind.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 44007FA1-3ABC-4935-BF52-4613AF0553A6
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/30/2018
ms.openlocfilehash: 9e4349b807c98e6f5cfbc55fa57153f99054d474
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732465"
---
# <a name="hello-android-quickstart"></a>Hallo, Android: Schnellstart

_In diesem zweiteiligen Leitfaden erstellen Sie Ihre erste Xamarin.Android-Anwendung – entweder mit Visual Studio oder mit Visual Studio für Mac. Außerdem entwickeln Sie ein Verständnis für die grundlegenden Aspekte der Entwicklung von Android-Anwendungen mit Xamarin. Währenddessen werden Ihnen die Tools, Konzepte und Schritte vorgestellt, die zum Erstellen und Bereitstellen einer Xamarin.Android-Anwendung erforderlich sind._

## <a name="hello-android-quickstart"></a>Hallo, Android: Schnellstart

In dieser exemplarischen Vorgehensweise erstellen Sie eine Anwendung, die eine vom Benutzer eingegebene alphanumerische Telefonnummer in eine numerische Telefonnummer übersetzt und diese dem Benutzer anzeigt. Die endgültige Anwendung sieht wie folgt aus:

[![Screenshot der App, wenn sie abgeschlossen ist.](hello-android-quickstart-images/intro-app-examples-sml.png)](hello-android-quickstart-images/intro-app-examples.png#lightbox)


## <a name="requirements"></a>Anforderungen

Für diese exemplarische Vorgehensweise benötigen Sie Folgendes:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-   Windows 7 oder höher

-   Visual Studio 2015 Professional oder höher

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

-   Die aktuelle Version von Visual Studio für Mac

-   OS X Yosemite oder höher

-----

In dieser exemplarischen Vorgehensweise wird davon ausgegangen, dass die neueste Version von Xamarin.Android auf der Plattform Ihrer Wahl installiert und aktiv ist. Eine Anleitung für die Installation von Xamarin.Android finden Sie unter [Xamarin.Android-Installation](~/android/get-started/installation/index.md).
Bevor Sie beginnen, laden Sie bitte [Xamarin-App-Symbole & Startbildschirme](https://github.com/xamarin/monodroid-samples/blob/master/Phoneword/Resources/XamarinAndroidIcons.zip?raw=true) herunter, und entpacken Sie es.

## <a name="configuring-emulators"></a>Konfigurieren von Emulatoren

Bei Verwendung des Google Android SDK-Emulators wird empfohlen, dass Sie den Emulator konfigurieren, um die Hardwarebeschleunigung verwenden zu können. Anweisungen zum Konfigurieren der Hardwarebeschleunigung finden Sie unter [Hardwarebeschleunigung für die Emulator-Leistung](~/android/get-started/installation/android-emulator/hardware-acceleration.md).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Wenn Sie den Visual Studio-Emulator für Android verwenden, muss Hyper-V auf Ihrem Computer aktiviert sein. Weitere Informationen zum Konfigurieren von Visual Studio-Android-Emulator finden Sie unter [Systemanforderungen für den Visual Studio-Emulator für Android](https://msdn.microsoft.com/en-us/library/mt228280.aspx).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

-----

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Starten Sie Visual Studio.  Klicken Sie auf **Datei > Neu > Projekt**, um ein neues Projekt zu erstellen.

Klicken Sie im Dialogfeld **Neues Projekt** auf die Vorlage **Android-App**.
Geben Sie dem neuen Projekt den Namen `Phoneword`. Klicken Sie auf **OK**, um das neue Projekt zu erstellen:

[![Neues Projekt ist Phoneword](hello-android-quickstart-images/vs/02-new-project-name-sml.w157.png)](hello-android-quickstart-images/vs/02-new-project-name.w157.png#lightbox)

### <a name="creating-the-layout"></a>Erstellen des Layouts

Nachdem das neue Projekt erstellt wurde, erweitern Sie den **Ressourcen**-Ordner und dann den **Layout**-Ordner im **Projektmappen-Explorer**.
Doppelklicken Sie auf **Main.axml**, um es in Android Designer zu öffnen. Dies ist die Layoutdatei für den App-Bildschirm:

[![Main.axml öffnen](hello-android-quickstart-images/vs/04-open-layout-sml.png)](hello-android-quickstart-images/vs/04-open-layout.png#lightbox)

Geben Sie in der **Toolbox** (der Bereich auf der linken Seite) `text` in das Suchfeld ein, und ziehen Sie ein **Text (groß)**-Widget auf die Entwurfsoberfläche (Bereich in der Mitte):

[![Ein großes Text-Widget hinzufügen](hello-android-quickstart-images/vs/04-large-text-sml.png)](hello-android-quickstart-images/vs/04-large-text.png#lightbox)

Wählen Sie das **Text (groß)**-Steuerelement auf der Entwurfsoberfläche aus, und verwenden Sie den **Eigenschaften**-Bereich, um die `text`-Eigenschaft des **Text (groß)**-Widgets zu `Enter a Phoneword:` zu ändern, wie hier gezeigt:

[![Legen Sie große Texteigenschaften fest](hello-android-quickstart-images/vs/05-enter-a-phoneword-sml.png)](hello-android-quickstart-images/vs/05-enter-a-phoneword.png#lightbox)

Ziehen Sie ein **Klartext**-Widget aus der **Toolbox** auf die Entwurfsoberfläche, und platzieren Sie es unterhalb des **Text (groß)**-Widgets:

[![Klartext-Widget hinzufügen](hello-android-quickstart-images/vs/06-plain-text-sml.png)](hello-android-quickstart-images/vs/06-plain-text.png#lightbox)

Wählen Sie das **Klartext**-Widget auf der Entwurfsoberfläche aus, und verwenden Sie den **Eigenschaften**-Bereich, um die `id`-Eigenschaft des **Klartext**-Widgets zu `@+id/PhoneNumberText` und die `text`-Eigenschaft zu `1-855-XAMARIN` zu ändern:

[![Legen Sie Klartexteigenschaften fest](hello-android-quickstart-images/vs/07-add-properties-sml.png)](hello-android-quickstart-images/vs/07-add-properties.png#lightbox)

Ziehen Sie eine **Schaltfläche** aus der **Toolbox** auf die Entwurfsoberfläche, und platzieren Sie sie unterhalb des **Klartext**-Widgets:

[![Ziehen Sie die Übersetzen-Schaltfläche auf die Entwurfsoberfläche](hello-android-quickstart-images/vs/08-drag-button-sml.png)](hello-android-quickstart-images/vs/08-drag-button.png#lightbox)

Wählen Sie die **Schaltfläche** auf der Entwurfsoberfläche aus, und verwenden Sie den **Eigenschaften**-Bereich, um die `id`-Eigenschaft der **Schaltfläche** zu `@+id/TranslateButton` und die `text`-Eigenschaft zu `Translate` zu ändern:

[![Eigenschaften der Übersetzen-Schaltfläche festlegen](hello-android-quickstart-images/vs/09-translate-button-sml.png)](hello-android-quickstart-images/vs/09-translate-button.png#lightbox)

Ziehen Sie aus der **Toolbox** ein **TextView**-Objekt auf die Entwurfsoberfläche, und platzieren Sie dieses unterhalb des Widgets **Schaltfläche**. Legen Sie die `id`-Eigenschaft des **TextView**-Objekts auf `@+id/TranslatedPhoneWord` fest, und ändern Sie `text` in eine leere Zeichenfolge:

[![Festlegen der TextView-Eigenschaften](hello-android-quickstart-images/vs/10-textview-properties-sml.png)](hello-android-quickstart-images/vs/10-textview-properties.png#lightbox)    

Speichern Sie Ihre Arbeit durch Drücken von **STRG+S**.

### <a name="writing-translation-code"></a>Schreiben von Übersetzungscode

Im nächste Schritt fügen Sie Code hinzu, um alphanumerische Telefonnummern in numerische Telefonnummern zu übersetzen. Fügen Sie dem Projekt eine neue Datei hinzu, indem Sie mit der rechten Maustaste auf das **Phoneword**-Projekt im **Projektmappen-Explorer** klicken, und wie unten gezeigt **Hinzufügen > Neue Datei...** auswählen:

[![Neues Element hinzufügen](hello-android-quickstart-images/vs/12-add-new-item-sml.png)](hello-android-quickstart-images/vs/12-add-new-item.png#lightbox)

Klicken Sie im Dialogfeld **Neues Element hinzufügen** auf **Visual C# > Code > Codedatei**, und nennen Sie die neue Datei **PhoneTranslator.cs**:

[![PhoneTranslator.cs hinzufügen](hello-android-quickstart-images/vs/14-add-class-sml.w157.png)](hello-android-quickstart-images/vs/14-add-class.w157.png#lightbox)

Dadurch wird eine neue leere C#-Klasse erstellt. Fügen Sie folgenden Code in diese Datei ein:

```csharp
using System.Text;
using System;
namespace Core
{
    public static class PhonewordTranslator
    {
        public static string ToNumber(string raw)
        {
            if (string.IsNullOrWhiteSpace(raw))
                return "";
            else
                raw = raw.ToUpperInvariant();

            var newNumber = new StringBuilder();
            foreach (var c in raw)
            {
                if (" -0123456789".Contains(c))
                {
                    newNumber.Append(c);
                }
                else
                {
                    var result = TranslateToNumber(c);
                    if (result != null)
                        newNumber.Append(result);
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
            if ("ABC".Contains(c))
                return 2;
            else if ("DEF".Contains(c))
                return 3;
            else if ("GHI".Contains(c))
                return 4;
            else if ("JKL".Contains(c))
                return 5;
            else if ("MNO".Contains(c))
                return 6;
            else if ("PQRS".Contains(c))
                return 7;
            else if ("TUV".Contains(c))
                return 8;
            else if ("WXYZ".Contains(c))
                return 9;
            return null;
        }
    }
}
```

Speichern Sie die Änderungen in der **PhoneTranslator.cs**-Datei durch Klicken auf **Datei > Speichern** (oder durch Drücken von **STRG + S**), und schließen Sie die Datei.

### <a name="wiring-up-the-interface"></a>Verknüpfen der Schnittstelle

Der nächste Schritt besteht im Hinzufügen von Code, um die Benutzeroberfläche zu verknüpfen, indem Sie zugrunde liegenden Code in die `MainActivity`-Klasse einfügen. Verknüpfen Sie zuerst die **Übersetzen**-Schaltfläche. Suchen Sie in der `MainActivity`-Klasse die Methode `OnCreate`. Im nächsten Schritt fügen Sie den Schaltflächencode in `OnCreate` unter den `base.OnCreate(bundle)`- und `SetContentView
(Resource.Layout.Main)`-Aufrufen hinzu. Ändern Sie zunächst den Vorlagencode, damit die `OnCreate`-Methode der folgenden Darstellung ähnelt:

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Widget;
using Android.OS;

namespace Phoneword
{
    [Activity (Label = "Phone Word", MainLauncher = true)]
    public class MainActivity : Activity
    {
        protected override void OnCreate (Bundle bundle)
        {
            base.OnCreate (bundle);

            // Set our view from the "main" layout resource
            SetContentView (Resource.Layout.Main);

            // New code will go here
        }
    }
}
```

Sie erhalten einen Verweis auf die Steuerelemente, die in der Layoutdatei über den Android Designer erstellt wurden. Fügen Sie in der `OnCreate`-Methode nach dem Aufruf von `SetContentView` folgenden Code hinzu:

```csharp
// Get our UI controls from the loaded layout
EditText phoneNumberText = FindViewById<EditText>(Resource.Id.PhoneNumberText);
TextView translatedPhoneWord = FindViewById<TextView>(Resource.Id.TranslatedPhoneWord);
Button translateButton = FindViewById<Button>(Resource.Id.TranslateButton);
```

Fügen Sie Code hinzu, der auf Benutzer reagiert, die auf die **Übersetzen**-Schaltfläche klicken.
Fügen Sie den folgenden Code zur `OnCreate`-Methode hinzu (nach den hinzugefügten Zeilen im vorherigen Schritt):

```csharp
// Add code to translate number
translateButton.Click += (sender, e) =>
{
    // Translate user's alphanumeric phone number to numeric
    string translatedNumber = Core.PhonewordTranslator.ToNumber(phoneNumberText.Text);
    if (string.IsNullOrWhiteSpace(translatedNumber))
    {
        translatedPhoneWord.Text = string.Empty;
    }
    else
    {
        translatedPhoneWord.Text = translatedNumber;
    }
};
```

Speichern Sie Ihre Arbeit durch Auswahl von **Datei > Alle speichern** (oder durch Drücken auf **STRG+UMSCHALT+S**), und erstellen Sie die Anwendung durch Auswahl von **Erstellen > Projektmappe neu erstellen** (oder durch Drücken auf **STRG+UMSCHALT+B**). 

Wenn Fehler auftreten, wiederholen Sie die vorherigen Schritte, und beheben Sie alle Fehler, bis die Anwendung erfolgreich erstellt wird. Wenn Sie einen Buildfehler wie _Resource does not exist in the current context_ (Ressource ist im aktuellen Kontext nicht vorhanden) erhalten, überprüfen Sie, ob der Name des Namespaces in **MainActivity.cs** dem Projektnamen (`Phoneword`) entspricht, und erstellen Sie dann die Projektmappe völlig neu. Wenn Sie weiterhin Buildfehler erhalten, stellen Sie sicher, dass Sie die neuesten Xamarin.Android-Updates installiert haben.

### <a name="setting-the-label-and-app-icon"></a>Festlegen der Bezeichnung und des App-Symbols

Sie sollten jetzt über eine funktionierende Anwendung verfügen &ndash; geben Sie ihr den letzten Schliff! Bearbeiten Sie in **MainActivity.cs** die `Label` für die `MainActivity`. Android zeigt `Label` am oberen Bildschirmrand, damit Benutzer wissen, wo sie sich in der Anwendung befinden.
Ändern Sie am oberen Rand der `MainActivity`-Klasse `Label` auf `Phone Word`, wie hier gezeigt:

```csharp
namespace Phoneword
{
    [Activity (Label = "Phone Word", MainLauncher = true)]
    public class MainActivity : Activity
    {
        ...
    }
}
```

Legen Sie nun das Anwendungssymbol fest. Standardmäßig stellt Visual Studio ein Standardsymbol für das Projekt bereit. Löschen Sie diese Dateien aus der Projektmappe, und ersetzen Sie diese durch ein anderes Symbol. Erweitern Sie den Ordner **Ressourcen** im **Lösungspad**. Beachten Sie, dass es fünf Ordner mit dem Präfix **mipmap-** gibt und dass jeder dieser Ordner eine einzelne **Icon.png**-Datei enthält:

[![mipmap-Ordner und Icon.png-Dateien](hello-android-quickstart-images/vs/21-mipmap-folders-sml.png)](hello-android-quickstart-images/vs/21-mipmap-folders.png#lightbox)

Es ist erforderlich, jede dieser Symboldateien aus dem Projekt zu löschen. Klicken Sie jeweils mit der rechten Maustaste auf die **Icon.png**-Dateien, und klicken Sie im Kontextmenü auf **Löschen**:
   
[![Löschen der Standarddatei „Icon.png“](hello-android-quickstart-images/vs/21-delete-icon-sml.png)](hello-android-quickstart-images/vs/21-delete-icon.png#lightbox)
   
Klicken Sie im Dialogfeld auf die Schaltfläche **Löschen**.

Laden Sie dann die [Xamarin-App-Symbole](https://github.com/xamarin/monodroid-samples/blob/master/Phoneword/Resources/XamarinAndroidIcons.zip?raw=true) herunter, und entpacken Sie diese. Diese ZIP-Datei enthält die Symbole für die Anwendung. Die Symbole sind visuell identisch, liegen aber in verschiedenen Auflösungen vor, sodass sie richtig auf verschiedenen Geräten mit verschiedenen Bildschirmdichten gerendert werden können.  Die Dateien müssen in das Xamarin.Android-Projekt kopiert werden. Klicken Sie im **Projektmappen-Explorer** in Visual Studio auf den Ordner **mipmap-hdpi**, und klicken Sie auf **Hinzufügen > Vorhandene Elemente**:

[![Hinzufügen von Dateien](hello-android-quickstart-images/vs/22-add-files-sml.png)](hello-android-quickstart-images/vs/22-add-files.png#lightbox)

Navigieren Sie vom Dialogfeld „Auswahl“ zum verpackten Verzeichnis der Xamarin-App-Symbole, und öffnen Sie den **mipmap-hdpi**-Ordner. Wählen Sie **Icon.png** aus, und klicken Sie auf **Hinzufügen**.

Wiederholen Sie diese Schritte für jeden der **mipmap**-Ordner, bis die Inhalte der **mipmap**-Ordner für Xamarin-App-Symbole in ihre Gegenstücke, die **mipmap**-Ordner im **Phoneword**-Projekt, kopiert werden.

Nachdem alle Symbole in das Xamarin.Android-Projekt kopiert wurden, öffnen Sie das Dialogfeld **Projektoptionen**, indem Sie mit der rechten Maustaste auf das Projekt im **Lösungspad** klicken. Klicken Sie auf **Erstellen > Android-Anwendung**, und wählen Sie **@mipmap/icon** aus dem Kombinationsfeld **Anwendungssymbol** aus:

[![Festlegen des Projektsymbols](hello-android-quickstart-images/vs/25-set-project-icon-sml.png)](hello-android-quickstart-images/vs/25-set-project-icon.png#lightbox)

### <a name="running-the-app"></a>Ausführen der App

Testen Sie die Anwendung am Schluss, indem Sie diese auf einem Android-Gerät oder auf einem Emulator ausführen und ein Phoneword übersetzen:

[![Screenshot der App, wenn sie abgeschlossen ist.](hello-android-quickstart-images/intro-app-examples-sml.png)](hello-android-quickstart-images/intro-app-examples.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Starten Sie Visual Studio für Mac im Ordner **Anwendungen** oder aus **Spotlight**. 

Klicken Sie auf **Neue Projektmappe…**, um ein neues Projekt zu erstellen.

Klicken Sie im Dialogfeld **Vorlage für neues Projekt auswählen** auf **Android > App**, und wählen Sie die **Android-App**-Vorlage aus. Klicken Sie auf **Weiter**.

[![Android App-Vorlage auswählen](hello-android-quickstart-images/xs/03-choose-template-sml.png)](hello-android-quickstart-images/xs/03-choose-template.png#lightbox)

Geben Sie Ihrer neuen Anwendung im Dialogfeld **Android-App konfigurieren** den Namen `Phoneword`, und klicken Sie auf **Weiter**.

[![Konfigurieren der Android-App](hello-android-quickstart-images/xs/04-configure-android-app-sml.png)](hello-android-quickstart-images/xs/04-configure-android-app.png#lightbox)

Belassen Sie im Dialogfeld **Configure your new Android App** (Neue Android-App konfigurieren) die Namen von Projektmappen und Projektdateien bei `Phoneword`, und klicken Sie auf **Erstellen**, um das Projekt zu erstellen.

### <a name="creating-the-layout"></a>Erstellen des Layouts

Nachdem das neue Projekt erstellt wurde, erweitern Sie den **Ressourcen**-Ordner und dann den **Layout**-Ordner im **Projektmappenpad**.
Doppelklicken Sie auf **Main.axml**, um sie in Android Designer zu öffnen. Dies ist die Layoutdatei für den Bildschirm, wenn diese im Android Designer angezeigt wird:

[![Öffnen von Main.axml](hello-android-quickstart-images/xs/05-open-layout-sml.png)](hello-android-quickstart-images/xs/05-open-layout.png#lightbox)

Wählen Sie **Hello World, Click Me!** (Hallo, Welt! Hier klicken!) aus **Schaltfläche** auf der Entwurfsoberfläche. Klicken Sie auf **Löschen**, um sie zu entfernen. 

Geben Sie in der **Toolbox** (der Bereich auf der rechten Seite) `text` in das Suchfeld ein, und ziehen Sie ein **Text (groß)**-Widget auf die Entwurfsoberfläche (Bereich in der Mitte):

[![Ein großes Text-Widget hinzufügen](hello-android-quickstart-images/xs/06-large-text-sml.png)](hello-android-quickstart-images/xs/06-large-text.png#lightbox)

Wählen Sie das **Text (groß)**-Widget auf der Entwurfsoberfläche aus, und verwenden Sie den **Eigenschaften**-Bereich, um die `Text`-Eigenschaft des **Text (groß)**-Widgets in `Enter a Phoneword:` zu ändern, wie hier gezeigt:

[![Festlegen großer Text-Widgeteigenschaften](hello-android-quickstart-images/xs/07-enter-a-phoneword-sml.png)](hello-android-quickstart-images/xs/07-enter-a-phoneword.png#lightbox)

Ziehen Sie danach ein **Klartext**-Widget aus der **Toolbox** auf die Entwurfsoberfläche, und platzieren Sie es unterhalb des **Text (groß)**-Widgets. Beachten Sie, dass Sie das Suchfeld zum Auffinden von Widgets nach Namen verwenden können:

[![Klartext-Widget hinzufügen](hello-android-quickstart-images/xs/08-plain-text-sml.png)](hello-android-quickstart-images/xs/08-plain-text.png#lightbox)

Wählen Sie das **Klartext**-Widget auf der Entwurfsoberfläche aus, und verwenden Sie das **Eigenschaftenpad**, um die `Id`-Eigenschaft des **Klartext**-Widgets zu `@+id/PhoneNumberText` und die `Text`-Eigenschaft zu `1-855-XAMARIN` zu ändern:

[![Festlegen von Klartext-Widgeteigenschaften](hello-android-quickstart-images/xs/09-add-properties-sml.png)](hello-android-quickstart-images/xs/09-add-properties.png#lightbox)

Ziehen Sie eine **Schaltfläche** aus der **Toolbox** auf die Entwurfsoberfläche, und platzieren Sie sie unterhalb des **Klartext**-Widgets:

[![Hinzufügen einer Schaltfläche](hello-android-quickstart-images/xs/10-drag-button-sml.png)](hello-android-quickstart-images/xs/10-drag-button.png#lightbox)

Wählen Sie die **Schaltfläche** auf der Entwurfsoberfläche aus, und verwenden Sie das **Eigenschaftenpad**, um die `Id`-Eigenschaft der **Schaltfläche** zu `@+id/TranslateButton` und die `Text`-Eigenschaft zu `Translate` zu ändern:

[![Als Übersetzen-Schaltfläche konfigurieren](hello-android-quickstart-images/xs/11-translate-button-sml.png)](hello-android-quickstart-images/xs/11-translate-button.png#lightbox)

Ziehen Sie aus der **Toolbox** ein **TextView**-Objekt auf die Entwurfsoberfläche, und platzieren Sie dieses unterhalb des Widgets **Schaltfläche**. Wenn **TextView** aktiviert ist, legen Sie die `id`-Eigenschaft von **TextView** auf `@+id/TranslatedPhoneWord` fest, und ändern Sie `text` in eine leere Zeichenfolge:

[![Festlegen der TextView-Eigenschaften](hello-android-quickstart-images/xs/12-textview-properties-sml.png)](hello-android-quickstart-images/xs/12-textview-properties.png#lightbox)    

Speichern Sie Ihre Arbeit durch Drücken von **&#8984; + S**.

### <a name="writing-translation-code"></a>Schreiben von Übersetzungscode

Fügen Sie jetzt Code hinzu, um alphanumerische Telefonnummern in numerische Telefonnummern zu übersetzen. Fügen Sie dem Projekt eine neue Datei hinzu, indem Sie auf das Zahnradsymbol neben dem **Phoneword**-Projekt im **Projektmappenpad** klicken und **Hinzufügen > Neue Datei...** auswählen:

[![Hinzufügen einer neuen Datei zum Projekt](hello-android-quickstart-images/xs/14-add-new-file-sml.png)](hello-android-quickstart-images/xs/14-add-new-file.png#lightbox)

Klicken Sie im Dialogfeld **Neue Datei** auf **Allgemein > Leere Klasse**, nennen Sie die neue Datei **PhoneTranslator**, und klicken Sie auf **Neu**. Dadurch wird eine neue leere C#-Klasse für uns erstellt.

Entfernen Sie den gesamten Vorlagencode aus der neuen Klasse, und ersetzen Sie ihn durch folgenden Code:

```csharp
using System.Text;
using System;
namespace Core
{
    public static class PhonewordTranslator
    {
        public static string ToNumber(string raw)
        {
            if (string.IsNullOrWhiteSpace(raw))
                return "";
            else
                raw = raw.ToUpperInvariant();

            var newNumber = new StringBuilder();
            foreach (var c in raw)
            {
                if (" -0123456789".Contains(c))
                {
                    newNumber.Append(c);
                }
                else
                {
                    var result = TranslateToNumber(c);
                    if (result != null)
                        newNumber.Append(result);
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
            if ("ABC".Contains(c))
                return 2;
            else if ("DEF".Contains(c))
                return 3;
            else if ("GHI".Contains(c))
                return 4;
            else if ("JKL".Contains(c))
                return 5;
            else if ("MNO".Contains(c))
                return 6;
            else if ("PQRS".Contains(c))
                return 7;
            else if ("TUV".Contains(c))
                return 8;
            else if ("WXYZ".Contains(c))
                return 9;
            return null;
        }
    }
}
```

Speichern Sie die Änderungen in der **PhoneTranslator.cs**-Datei durch Klicken auf **Datei > Speichern** (oder durch Drücken von **&#8984; + S**), und schließen Sie die Datei. Stellen Sie durch erneutes Erstellen der Projektmappe sicher, dass keine Kompilierungsfehler vorhanden sind.

### <a name="wiring-up-the-interface"></a>Verknüpfen der Schnittstelle

Der nächste Schritt besteht im Hinzufügen von Code, um die Benutzeroberfläche zu verknüpfen, indem Sie zugrunde liegenden Code in die `MainActivity`-Klasse einfügen.
Doppelklicken Sie auf **MainActivity.cs** im **Lösungspad**, um sie zu öffnen.

Fügen Sie zunächst einen Ereignishandler zur Schaltfläche **Übersetzen** hinzu. Suchen Sie in der `MainActivity`-Klasse die Methode `OnCreate`. Fügen Sie den Schaltflächencode in `OnCreate` unter den `base.OnCreate(bundle)`- und `SetContentView (Resource.Layout.Main)`-Aufrufen hinzu. Entfernen Sie den Vorlagencode für die Verarbeitung der Schaltflächen, damit die `OnCreate`-Methode der folgenden Darstellung ähnelt:

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Runtime;
using Android.Views;
using Android.Widget;
using Android.OS;

namespace Phoneword
{
    [Activity (Label = "Phone Word", MainLauncher = true)]
    public class MainActivity : Activity
    {
        protected override void OnCreate (Bundle bundle)
        {
            base.OnCreate (bundle);

            // Set our view from the "main" layout resource
            SetContentView (Resource.Layout.Main);

            // Our code will go here
        }
    }
}
```

Sie erhalten als nächstes einen Verweis auf die Steuerelemente, die in der Layoutdatei über den Android Designer erstellt wurden. Fügen Sie in der `OnCreate`-Methode nach dem Aufruf von `SetContentView` folgenden Code hinzu:

```csharp
// Get our UI controls from the loaded layout
EditText phoneNumberText = FindViewById<EditText>(Resource.Id.PhoneNumberText);
TextView translatedPhoneWord = FindViewById<TextView>(Resource.Id.TranslatedPhoneWord);
Button translateButton = FindViewById<Button>(Resource.Id.TranslateButton);
```

Fügen Sie Code hinzu, der auf den Benutzer reagiert, wenn dieser auf die **Übersetzen**-Schaltfläche klickt. Fügen Sie dazu den folgenden Code zur `OnCreate`-Methode hinzu (nach den Zeilen, die im letzten Schritt hinzugefügt wurden):

```csharp
// Add code to translate number
string translatedNumber = string.Empty;

translateButton.Click += (sender, e) =>
{
    // Translate user's alphanumeric phone number to numeric
    translatedNumber = PhonewordTranslator.ToNumber(phoneNumberText.Text);
    if (string.IsNullOrWhiteSpace(translatedNumber))
    {
        translatedPhoneWord.Text = string.Empty;
    }
    else
    {
        translatedPhoneWord.Text = translatedNumber;
    }
};
```

Speichern Sie Ihre Arbeit, und erstellen Sie die Anwendung, indem Sie **Erstellen > Alle erstellen** auswählen (oder durch Drücken von **& #8984; + B**). Wenn die Anwendung kompiliert wird, erhalten Sie eine Erfolgsmeldung am oberen Rand von Visual Studio für Mac:

Wenn Fehler auftreten, wiederholen Sie die vorherigen Schritte, und beheben Sie alle Fehler, bis die Anwendung erfolgreich erstellt wird. Wenn Sie einen Buildfehler wie _Resource does not exist in the current context_ (Ressource ist im aktuellen Kontext nicht vorhanden) erhalten, überprüfen Sie, ob der Name des Namespaces in **MainActivity.cs** dem Projektnamen (`Phoneword`) entspricht, und erstellen Sie dann die Projektmappe völlig neu. Wenn weiterhin Buildfehler auftreten, stellen Sie sicher, dass Sie die neuesten Updates für Xamarin.Android und Visual Studio für Mac installiert haben.

### <a name="setting-the-label-and-app-icon"></a>Festlegen der Bezeichnung und des App-Symbols

Da Sie jetzt über eine funktionierende Anwendung verfügen, geben Sie ihr den letzten Schliff! Starten Sie durch Bearbeiten der `Label` für `MainActivity`.
Android zeigt `Label` am oberen Bildschirmrand, damit Benutzer wissen, wo sie sich in der Anwendung befinden. Ändern Sie am oberen Rand der `MainActivity`-Klasse `Label` auf `Phone Word`, wie hier gezeigt:

```csharp
namespace Phoneword
{
    [Activity (Label = "Phone Word", MainLauncher = true)]
    public class MainActivity : Activity
    {
        ...
    }
}
```

Legen Sie nun das Anwendungssymbol fest. Standardmäßig stellt Visual Studio für Mac ein Standardsymbol für das Projekt bereit. Löschen Sie diese Dateien aus der Projektmappe, und ersetzen Sie diese durch ein anderes Symbol. Erweitern Sie den Ordner **Ressourcen** im **Lösungspad**. Beachten Sie, dass es fünf Ordner mit dem Präfix **mipmap-** gibt und dass jeder dieser Ordner eine einzelne **Icon.png**-Datei enthält:

[![mipmap-Ordner und Icon.png-Dateien](hello-android-quickstart-images/xs/23-mipmap-folders-sml.png)](hello-android-quickstart-images/xs/23-mipmap-folders.png#lightbox)

Es ist erforderlich, jede dieser Symboldateien aus dem Projekt zu löschen. Klicken Sie jeweils mit der rechten Maustaste auf die **Icon.png**-Dateien, und klicken Sie im Kontextmenü auf **Entfernen**:

[![Löschen der Standarddatei „Icon.png“](hello-android-quickstart-images/xs/23-delete-icon-sml.png)](hello-android-quickstart-images/xs/23-delete-icon.png#lightbox)

Klicken Sie im Dialogfeld auf die Schaltfläche **Löschen**.

Laden Sie dann die [Xamarin-App-Symbole](https://github.com/xamarin/monodroid-samples/blob/master/Phoneword/Resources/XamarinAndroidIcons.zip?raw=true) herunter, und entpacken Sie diese. Diese ZIP-Datei enthält die Symbole für die Anwendung. Die Symbole sind visuell identisch, liegen aber in verschiedenen Auflösungen vor, sodass sie richtig auf verschiedenen Geräten mit verschiedenen Bildschirmdichten gerendert werden können.  Die Dateien müssen in das Xamarin.Android-Projekt kopiert werden. Klicken Sie im **Lösungspad** in Visual Studio für Mac auf den Ordner **mipmap-hdpi**, und klicken Sie auf **Hinzufügen > Dateien hinzufügen**:

[![Hinzufügen von Dateien](hello-android-quickstart-images/xs/24-add-files-sml.png)](hello-android-quickstart-images/xs/24-add-files.png#lightbox)

Navigieren Sie vom Dialogfeld „Auswahl“ zum verpackten Verzeichnis der Xamarin-App-Symbole, und öffnen Sie den **mipmap-hdpi**-Ordner. Wählen Sie **Icon.png** aus, und klicken Sie auf **Öffnen**.

Wählen Sie im Dialogfeld **Datei zum Ordner hinzufügen** **Copy the file into the directory** (Datei in das Verzeichnis kopieren) aus, und klicken Sie auf **OK**:

[![Dialogfeld „Datei in das Verzeichnis kopieren“](hello-android-quickstart-images/xs/26-copy-to-directory-sml.png)](hello-android-quickstart-images/xs/26-copy-to-directory.png#lightbox)

Wiederholen Sie diese Schritte für jeden der **mipmap**-Ordner, bis die Inhalte der **mipmap**-Ordner für Xamarin-App-Symbole in ihre Gegenstücke, die **mipmap**-Ordner im **Phoneword**-Projekt, kopiert werden.

Nachdem alle Symbole in das Xamarin.Android-Projekt kopiert wurden, öffnen Sie das Dialogfeld **Projektoptionen**, indem Sie mit der rechten Maustaste auf das Projekt im **Lösungspad** klicken. Klicken Sie auf **Erstellen > Android-Anwendung**, und wählen Sie **@mipmap/icon** aus dem Kombinationsfeld **Anwendungssymbol** aus:

[![Festlegen des Projektsymbols](hello-android-quickstart-images/xs/28-set-project-icon-sml.png)](hello-android-quickstart-images/xs/28-set-project-icon.png#lightbox)

### <a name="running-the-app"></a>Ausführen der App

Testen Sie die Anwendung am Schluss, indem Sie diese auf einem Android-Gerät oder auf einem Emulator ausführen und ein Phoneword übersetzen:

[![Screenshot der App, wenn sie abgeschlossen ist.](hello-android-quickstart-images/intro-app-examples-sml.png)](hello-android-quickstart-images/intro-app-examples.png#lightbox)

-----

Herzlichen Glückwunsch, Sie haben Ihre erste Xamarin.Android-Anwendung fertiggestellt!
Jetzt ist es an der Zeit, sich die Tools und Fertigkeiten, die Sie gerade gelernt haben, genau anzusehen. Als Nächstes lesen Sie [Hello, Android Deep Dive (Hallo, Android: Ausführliche Erläuterungen)](~/android/get-started/hello-android/hello-android-deepdive.md).


## <a name="related-links"></a>Verwandte Links

- [Xamarin Android-App-Symbole (ZIP)](https://github.com/xamarin/monodroid-samples/blob/master/Phoneword/Resources/XamarinAndroidIcons.zip?raw=true)
- [Phoneword (Beispiel)](https://developer.xamarin.com/samples/monodroid/Phoneword)
