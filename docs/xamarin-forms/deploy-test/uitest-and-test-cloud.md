---
title: "Automatisierte Tests für Xamarin.Forms mit Xamarin.UITest und App Center"
description: "Die UITest-Komponente von Xamarin kann mit Xamarin.Forms eingesetzt werden, um UI-Tests zu schreiben, die auf Hunderten von Geräten in der Cloud laufen."
ms.topic: article
ms.prod: xamarin
ms.assetid: b674db3d-c526-4e31-a9f4-b6d6528ce7a9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/31/2016
ms.openlocfilehash: 78788524c1afdda127762049018ca769926f729e
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="automate-xamarinforms-testing-with-xamarinuitest-and-app-center"></a>Automatisierte Tests für Xamarin.Forms mit Xamarin.UITest und App Center

_Die UITest-Komponente von Xamarin kann mit Xamarin.Forms eingesetzt werden, um UI-Tests zu schreiben, die auf Hunderten von Geräten in der Cloud ausgeführt werden._

## <a name="overview"></a>Übersicht

**[App Center Test](/appcenter/test-cloud/)** ermöglicht Entwicklern, automatisierte Tests von Benutzeroberflächen für iOS- und Android-Apps zu schreiben. Es sind nur einige kleinere Anpassungen nötig, damit Xamarin.Forms-Apps mit Xamarin.UITest getestet werden können: Unter anderem muss derselbe Testcode verwendet werden. Im Folgenden werden spezifische Tipps gegeben, die dabei helfen sollen, Xamarin.UITest mit Xamarin.Forms auszuführen.

In diesem Leitfaden werden Kenntnisse im Umgang mit Xamarin.UITest vorausgesetzt. Wenn Sie sich mit Xamarin.UITest vertraut machen möchten, empfehlen wir Ihnen folgende Leitfäden:

- [Einführung in App Center Test](/appcenter/test-cloud/)
- [Introduction to UITest (Einführung in UITest)](/appcenter/test-cloud/preparing-for-upload/uitest/)

Sobald Sie ein UI-Testprojekt zu einer Xamarin.Forms-Projektmappe hinzugefügt haben, sind sowohl für Xamarin.Android- als auch für Xamarin.iOS-Anwendungen dieselben Schritte zu befolgen, um einen Test schreiben und ausführen zu können.

## <a name="requirements"></a>Anforderungen

Vergewissern Sie sich anhand der Informationen unter [Xamarin.UITest](/appcenter/test-cloud/uitest/), dass Ihr Projekt für automatisierte UI-Tests bereit ist.

## <a name="adding-uitest-support-to-xamarinforms-apps"></a>Hinzufügen von UITest-Support zu Xamarin.Forms-Apps

UITest automatisiert die Benutzeroberfläche, indem es Steuerelemente auf dem Bildschirm aktiviert und überall dort Eingaben ausführt, wo ein Benutzer normalerweise mit der Anwendung interagieren würde. Zum Aktivieren von Tests, die auf *Schaltflächen drücken* oder *Text in ein Feld einfügen* können, muss für den Testcode eine Möglichkeit bestehen, die Steuerelemente auf dem Bildschirm zu identifizieren.

Zum Aktivieren des UITest-Codes, der auf Steuerelemente verweisen soll, benötigt jedes Steuerelement einen eindeutigen Bezeichner. Für Xamarin.Forms wird empfohlen, wie im folgenden Beispiel die `AutomationId`-Eigenschaft zu verwenden, um diesen Bezeichner festzulegen:

```csharp
var b = new Button {
    Text = "Click me",
    AutomationId = "MyButton"
};
var l = new Label {
    Text = "Hello, Xamarin.Forms!",
    AutomationId = "MyLabel"
};
```

Die `AutomationId`-Eigenschaft kann auch auf in XAML festgelegt werden:

```xaml
<Button x:Name="b" AutomationId="MyButton" Text="Click me"/>
<Label x:Name="l" AutomationId="MyLabel" Text="Hello, Xamarin.Forms!" />
```

Eine eindeutige `AutomationId` sollte zu allen Steuerelementen hinzugefügt werden, für die Tests erforderlich sind (einschließlich Schaltflächen, Texteingaben und Bezeichnungen, deren Wert möglicherweise abgefragt werden muss).

> [!NOTE]
> Beachten Sie, dass eine `InvalidOperationException` ausgelöst wird, wenn es einen Versuch geben sollte, die `AutomationId`-Eigenschaft eines `Element` mehr als einmal festzulegen.

### <a name="ios-application-project"></a>Anwendungsprojekt für iOS

Zum Ausführen von Tests unter iOS muss das [Xamarin Test Cloud Agent NuGet package (Xamarin Test Cloud Agent NuGet-Paket)](https://www.nuget.org/packages/Xamarin.TestCloud.Agent/) zu dem Projekt hinzugefügt werden. Wenn dies geschehen ist, kopieren Sie den folgenden Code in die `AppDelegate.FinishedLaunching`-Methode:

```csharp
#if ENABLE_TEST_CLOUD
// requires Xamarin Test Cloud Agent
Xamarin.Calabash.Start();
#endif
```

Die Calabash-Assembly verwendet nicht öffentliche Apple-API, wodurch Apps vom App Store abgelehnt werden. Allerdings entfernt der Xamarin.iOS-Linker die Calabash-Assembly von der endgültigen IPA, wenn der Code nicht explizit auf sie verweist.

> [!NOTE]
>  Releasebuilds verfügen nicht über die Compilervariable `ENABLE_TEST_CLOUD`, weshalb die Calabash-Assembly aus dem App Bundle entfernt wird. Demgegenüber haben Debugbuilds Compilerdirektive definiert, wodurch der Linker daran gehindert wird, die Assembly zu entfernen.

Im folgenden Screenshot wird die Compilervariable `ENABLE_TEST_CLOUD` gezeigt, die für Debugbuilds festgelegt ist:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](uitest-and-test-cloud-images/12-compiler-directive-vs.png "Buildoptionen")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

![](uitest-and-test-cloud-images/11-compiler-directive-xs.png "Compileroptionen")

-----

### <a name="android-application-project"></a>Anwendungsprojekt für Android

Im Gegensatz zu iOS-Projekten benötigen Android-Projekte keinen besonderen Startcode.

## <a name="writing-uitests"></a>Schreiben von UITests

Weitere Informationen zum Schreiben von UITests finden Sie in der [UITest-Dokumentation](/appcenter/test-cloud/preparing-for-upload/uitest/). Im Folgenden wird Schritt für Schritt zusammengefasst, wie die [Xamarin.Forms-Demo **UsingUITest**](https://developer.xamarin.com/samples/xamarin-forms/UsingUITest/) erstellt wird.

### <a name="use-automationid-in-the-xamarinforms-ui"></a>Verwenden von AutomationID in der Xamarin.Forms-UI

Bevor UITests geschrieben werden können, muss die Benutzeroberfläche der Xamarin.Forms-Anwendung skriptfähig sein. Stellen Sie sicher, dass sämtliche Steuerelemente in der Benutzeroberfläche eine `AutomationId` haben, damit darauf im Testcode verwiesen werden kann.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

### <a name="adding-a-uitest-project-to-a-new-solution"></a>Hinzufügen eines UITest-Projekts zu einer neuen Projektmappe

Wenn Sie mit Visual Studio für Mac ein neues Xamarin.Forms-Projekt erstellen, können Sie auch ein neues UITest-Projekt zu der Projektmappe hinzufügen, indem Sie **Xamarin Test Cloud: Fügen Sie ein automatisiertes UI-Testprojekt hinzu** auswählen:

![](uitest-and-test-cloud-images/01-new-solution-xs.png "Konfigurieren Ihres neuen Projekts")

Die neue Projektmappe wird automatisch konfiguriert, um Xamarin.UITests für die Xamarin.Forms-Anwendung auszuführen.

-----

#### <a name="referring-to-the-automationid-in-uitests"></a>Verweisen auf die AutomationID in UITests

Wenn Sie UITests schreiben, wir der `AutomationId`-Wert auf jeder Plattform anders dargestellt:

- Unter **iOS** wird das `id`-Feld verwendet.
- Unter **Android** wird das `label`-Feld verwendet.

Verwenden Sie die `Marked`-Testabfrage, um plattformübergreifende UITests zu schreiben, die sowohl unter iOS als auch unter Android die `AutomationId` finden:

```csharp
app.Query(c=>c.Marked("MyButton"))
```

Die kürzere Form `app.Query("MyButton")` funktioniert ebenfalls.

### <a name="adding-a-uitest-project-to-an-existing-solution"></a>Hinzufügen eines UITest-Projekts zu einer existierenden Projektmappe

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

In Visual Studio gibt es eine Vorlage, die beim Hinzufügen eines Xamarin.UITest-Projekts zu einer bestehenden Xamarin.Forms-Projektmappe helfen soll:

1. Klicken Sie mit der rechten Maustaste auf die Projektmappe, und wählen Sie anschließend **Datei > Neues Projekt** aus.
1. Wählen Sie aus den **Visual C#**-Vorlagen eine **Testkategorie** aus. Klicken Sie auf die Vorlage **UI Test App > Cross-Platform** (UI Test-App > Plattformübergreifend):

    ![](uitest-and-test-cloud-images/08-new-uitest-project-vs.png "Hinzufügen eines neuen Projekts")

    So fügen Sie ein neues Projekt mit den NuGet-Paketen **NUnit**, **Xamarin.UITest** und **NUnitTestAdapter** zu der Projektmappe hinzu:

    ![](uitest-and-test-cloud-images/09-new-uitest-project-xs.png "NuGet-Paket-Manager")

    **NUnitTestAdapter** ist ein Test Runner eines Drittanbieters, mit dem Visual Studio NUnit-Tests ausführen kann.

    Das neue Projekt umfasst außerdem zwei Klassen. **AppInitializer** enthält Code zum Initialisieren und Starten von Tests. Die andere Klasse, **Tests**, enthält Codebausteine zum Starten von UITests.

1. Fügen Sie einen Projektverweis aus dem UITest-Projekt zu dem Xamarin.Android-Projekt hinzu:

    ![](uitest-and-test-cloud-images/10-test-apps-vs.png "Verweis-Manager für Projekte")

    Dadurch kann **NUnitTestAdapter** über Visual Studio UITests für die Android-App ausführen.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Es besteht die Möglichkeit, ein neues Xamarin.UI-Testprojekt manuell zu einer bereits bestehenden Projektmappe hinzuzufügen:

1. Fügen Sie ein neues Projekt hinzu, indem Sie zunächst die Projektmappe auswählen und auf **Datei > Neues Projekt hinzufügen** klicken. Klicken Sie im Dialogfeld **Neues Projekt** auf **Cross-platform > Tests > Xamarin Test Cloud > UI Test App** (Plattformübergreifend > Tests > Xamarin Test Cloud > UI Test-App):

    ![](uitest-and-test-cloud-images/02-new-uitest-project-xs.png "Wählen einer Vorlage")

    So fügen Sie ein neues Projekt hinzu, das bereits die **NUnit**- und **Xamarin.UITest**-NuGet-Pakete in der Projektmappe enthält:

    ![](uitest-and-test-cloud-images/03-new-uitest-project-xs.png "NuGet-Pakete für Xamarin UITest")

    Das neue Projekt umfasst außerdem zwei Klassen. **AppInitializer** enthält Code zum Initialisieren und Starten von Tests. Die andere Klasse, **Tests**, enthält Codebausteine zum Starten von UITests.

1. Wählen Sie **View > Pads > Unit Tests** (Ansicht > Pads > Komponententests), damit das Komponententestpad angezeigt wird. Erweitern Sie **UsingUITest > UsingUITest.UITests > Test Apps** (UsingUITest > UsingUITest.UITests > Apps testen):

    ![](uitest-and-test-cloud-images/04-unit-test-pad-xs.png "Komponententestpad")

1. Klicken Sie mit der rechten Maustaste zunächst auf **Test Apps** (Apps testen) und anschließend auf **Add App Project** (App-Projekt hinzufügen). Wählen Sie dann in dem erscheinenden Dialogfeld iOS- und Android-Projekte aus:

    ![](uitest-and-test-cloud-images/05-add-test-apps-xs.png "Dialogfeld „Apps testen“")

    Das Pad **Unit Test** (Komponententest) sollte nun über einen Verweis auf iOS- und Android-Projekte verfügen. Dadurch kann der Test Runner von Visual Studio für Mac UITests lokal für beide Xamarin.Forms-Projekte ausführen.

#### <a name="adding-uitest-to-the-ios-app"></a>Hinzufügen von UITests zur iOS-App

Bevor Xamarin.UITest funktionieren kann, müssen einige zusätzliche Änderungen an der iOS-Anwendung vorgenommen werden:

1. Fügen Sie das **Xamarin Test Cloud Agent** NuGet-Paket hinzu. Klicken Sie mit der rechten Maustaste auf **Packages** (Pakete)und anschließend auf **Add Packages** (Pakete hinzufügen), suchen Sie nach „NuGet“ für den **Xamarin Test Cloud Agent**, und fügen Sie es zu dem Xamarin.iOS-Projekt hinzu:

    ![](uitest-and-test-cloud-images/07-add-test-cloud-agent-xs.png "Hinzufügen von NuGet-Paketen")

1. Bearbeiten Sie die `FinishedLaunching`-Methode der **AppDelegate**-Klasse (Klasse des Anwendungsdelegaten), um den Xamarin Test Cloud Agent beim Start der iOS-Anwendung zu initialisieren und die `AutomationId`-Eigenschaft in der Ansicht festzulegen. Die `FinishedLaunching`-Methode sollte in etwa dem folgenden Codebeispiel entsprechen:

```csharp
public override bool FinishedLaunching(UIApplication app, NSDictionary options)
{
        #if ENABLE_TEST_CLOUD
        Xamarin.Calabash.Start();
        #endif

        global::Xamarin.Forms.Forms.Init();

        LoadApplication(new App());

        return base.FinishedLaunching(app, options);
}
```

-----

Nachdem Sie Xamarin.UITest zu der Xamarin.Forms-Projektmappe hinzugefügt haben, können Sie UITests erstellen, sie lokal ausführen und an die Xamarin Test Cloud senden.

## <a name="summary"></a>Zusammenfassung

Xamarin.Forms-Anwendungen können einfach mit **Xamarin.UITest** getestet werden, indem einfache Mechanismen verwendet werden, sodass die `AutomationId` als eindeutiger Ansichtsbezeichner für die Testautomatisierung verfügbar gemacht wird. Sobald Sie ein UI-Testprojekt zu einer Xamarin.Forms-Projektmappe hinzugefügt haben, sind sowohl für Xamarin.Android- als auch für Xamarin.iOS-Anwendungen dieselben Schritte zu befolgen, um einen Test schreiben und ausführen zu können.

Weitere Informationen zum Senden von Tests an App Center Test finden Sie unter [Submitting UITests](/appcenter/test-cloud/preparing-for-upload/uitest/) (Senden von UITests). Weitere Informationen zu UITest finden Sie in der [App Center Test-Dokumentation](/appcenter/test-cloud/).


## <a name="related-links"></a>Verwandte Links

- [UITestSample](https://developer.xamarin.com/samples/xamarin-forms/UsingUITest/)
- [Xamarin.Forms Samples (Beispiele für Xamarin.Forms)](https://github.com/xamarin/xamarin-forms-samples)
- [App Center Test (App Center-Test)](/appcenter/test-cloud/)
- [Xamarin.UITest](/appcenter/test-cloud/uitest/)
- [NUnit](http://www.nunit.org)
