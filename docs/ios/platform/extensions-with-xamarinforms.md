---
title: Wieder verwenden von xamarin. Forms-Seiten in einer IOS-Erweiterung
description: In diesem Dokument werden IOS-Erweiterungen und die Verwendung von xamarin. Forms-Seiten in diesem Dokument beschrieben. Es enthält eine exemplarische Vorgehensweise zum Erstellen einer Erweiterung, integrieren von xamarin. Forms und Rendern einer einfachen ContentPage in einem IOS-Erweiterungsprojekt.
ms.prod: xamarin
ms.assetid: 50076FFD-3EF6-41CC-BC7E-210DE3958F5B
ms.technology: xamarin-ios
author: alexeystrakh
ms.author: alstrakh
ms.date: 05/13/2020
ms.openlocfilehash: 4006bb3ef82264b5a7a6d764719bee81a8ce78a1
ms.sourcegitcommit: 5bc37b384706bfbf5bf8e5b1288a34ddd0f3303b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/15/2020
ms.locfileid: "83418073"
---
# <a name="reuse-xamarinforms-pages-in-an-ios-extension"></a>Wieder verwenden von xamarin. Forms-Seiten in einer IOS-Erweiterung

IOS-Erweiterungen ermöglichen es Ihnen, ein vorhandenes Systemverhalten anzupassen, indem Sie eine zusätzliche Funktionalität hinzufügen, die [von IOS-und macOS-Erweiterungs Punkten vordefiniert](https://developer.apple.com/library/archive/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214-CH20-SW2)wird, z. b. benutzerdefinierte Kontext Aktionen, Kenn Wort AutoFill, eingehende Aufruf Filter, Modifizierer Xamarin. IOS unterstützt Extensions, und [Dieses Handbuch](https://docs.microsoft.com/xamarin/ios/platform/extensions) führt Sie durch die Erstellung einer IOS-Erweiterung mithilfe von xamarin-Tools.

Erweiterungen werden als Teil einer Container-App verteilt und von einem bestimmten Erweiterungs Punkt in einer Host-App aktiviert. Bei der Container-App handelt es sich in der Regel um eine einfache IOS-Anwendung, die einem Benutzerinformationen über die Erweiterung, die Aktivierung und die Verwendung bereitstellt. Es gibt drei Hauptansätze für die gemeinsame Nutzung von Code zwischen einer Erweiterung und einer Container-App:

1. Häufiges IOS-Projekt.

    Sie können den gesamten freigegebenen Code zwischen dem Container und der Erweiterung in eine freigegebene IOS-Bibliothek einfügen und aus beiden Projekten auf die Bibliothek verweisen. Normalerweise enthält die freigegebene Bibliothek Native uiviewcontrollers und muss eine xamarin. IOS-Bibliothek sein.

1. Datei Verknüpfungen.

    In einigen Fällen stellt die Container-App den größten Teil der Funktionalität bereit, während die Erweiterung einen einzelnen Renderern muss `UIViewController` . Mit wenigen Dateien, die freigegeben werden können, ist es üblich, der Erweiterungs-APP einen Datei Link aus der Datei hinzuzufügen, die sich in der Container-App befindet.

1. Gängiges xamarin. Forms-Projekt.

    Wenn App-Seiten bereits mit einer anderen Plattform, z. b. Android, mit dem xamarin. Forms-Framework gemeinsam genutzt werden, besteht der gängige Ansatz darin, die erforderlichen Seiten nativ im Erweiterungsprojekt neu zu implementieren, da die IOS-Erweiterung mit nativen uiviewcontrollers und nicht mit xamarin. Forms-Seiten funktioniert. Sie müssen zusätzliche Schritte ausführen, um xamarin. Forms in der IOS-Erweiterung zu verwenden, die unten erläutert werden.

## <a name="xamarinforms-in-an-ios-extension-project"></a>Xamarin. Forms in einem IOS-Erweiterungsprojekt

Die Möglichkeit, xamarin. Forms in einem systemeigenen Projekt zu verwenden, wird über [native Formulare](~/xamarin-forms/platform/native-forms.md)bereitgestellt. Dadurch können von `ContentPage` abgeleitete Seiten direkt zu nativen xamarin. IOS-Projekten hinzugefügt werden. Die `CreateViewController` Erweiterungsmethode konvertiert eine Instanz einer xamarin. Forms-Seite in eine native `UIViewController` , die als regulärer Controller verwendet oder geändert werden kann. Da eine IOS-Erweiterung eine besondere Art eines systemeigenen IOS-Projekts ist, können Sie hier auch **native Formulare** verwenden.

> [!IMPORTANT]
> Es gibt viele [bekannte Einschränkungen](https://docs.microsoft.com/xamarin/ios/platform/extensions#limitations) für IOS-Erweiterungen. Obwohl Sie xamarin. Forms in einer IOS-Erweiterung verwenden können, sollten Sie dies sehr sorgfältig durchführen, indem Sie die Speicherauslastung und die Startzeit überwachen. Andernfalls kann die Erweiterung von IOS beendet werden, ohne dass diese ordnungsgemäß verarbeitet werden muss.

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

In dieser exemplarischen Vorgehensweise erstellen Sie eine xamarin. Forms-Anwendung, eine xamarin. IOS-Erweiterung und verwenden freigegebenen Code im Erweiterungsprojekt erneut:

1. Öffnen Sie Visual Studio für Mac, erstellen Sie ein neues xamarin. Forms-Projekt mithilfe der Vorlage **leere Forms-App** , und benennen Sie es **formsshareextension**:

    ![Projekt erstellen](extensions-xf-images/1.walkthrough-createproject.png)

1. Ersetzen Sie in **formsshareextension/MainPage. XAML**den Inhalt durch das folgende Layout:

    ```xaml
    <?xml version="1.0" encoding="utf-8" ?>
    <ContentPage
        x:Class="FormsShareExtension.MainPage"
        xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:d="http://xamarin.com/schemas/2014/forms/design"
        xmlns:local="clr-namespace:FormsShareExtension"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        x:DataType="local:MainPageViewModel"
        BackgroundColor="Orange"
        mc:Ignorable="d">
        <ContentPage.BindingContext>
            <local:MainPageViewModel Message="Hello from Xamarin.Forms!" />
        </ContentPage.BindingContext>
        <StackLayout HorizontalOptions="Center" VerticalOptions="Center">
            <Label
                Margin="20"
                Text="{Binding Message}"
                VerticalOptions="CenterAndExpand" />
            <Button Command="{Binding DoCommand}" Text="Do the job!" />
        </StackLayout>
    </ContentPage>
    ```

1. Fügen Sie dem Projekt " **formsshareextension** " eine neue Klasse mit dem Namen " **mainpageviewmode** " hinzu, und ersetzen Sie den Inhalt der Klasse durch den folgenden Code:

    ```csharp
    using System;
    using System.ComponentModel;
    using System.Windows.Input;
    using Xamarin.Forms;

    namespace FormsShareExtension
    {
        public class MainPageViewModel : INotifyPropertyChanged
        {
            public event PropertyChangedEventHandler PropertyChanged;

            private string _message;
            public string Message
            {
                get { return _message; }
                set
                {
                    if (_message != value)
                    {
                        _message = value;
                        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(nameof(Message)));
                    }
                }
            }

            private ICommand _doCommand;
            public ICommand DoCommand
            {
                get { return _doCommand; }
                set
                {
                    if(_doCommand != value)
                    {
                        _doCommand = value;
                        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(nameof(DoCommand)));
                    }
                }
            }

            public MainPageViewModel()
            {
                DoCommand = new Command(OnDoCommandExecuted);
            }

            private void OnDoCommandExecuted(object state)
            {
                Message = $"Job {Environment.TickCount} has been completed!";
            }
        }
    }
    ```

    Der Code wird für alle Plattformen freigegeben und wird auch von einer IOS-Erweiterung verwendet.

1. Klicken Sie im projektmappenpad mit der rechten Maustaste auf die Projekt Mappe, wählen Sie **> neues Projekt hinzufügen > IOS > Erweiterung > Aktions Erweiterung**aus, nennen Sie Sie **myaction** , **und drücken Sie**

    ![Erweiterung erstellen](extensions-xf-images/2.walkthrough-createextension.png)

1. Um xamarin. Forms in der IOS-Erweiterung und dem freigegebenen Code zu verwenden, müssen Sie erforderliche Verweise hinzufügen:

    - Klicken Sie mit der rechten Maustaste auf die IOS-Erweiterung, und wählen Sie **Verweise > Verweis > Projekte hinzufügen > formsshareextension** und **OK**

    - Klicken Sie mit der rechten Maustaste auf die IOS-Erweiterung, wählen Sie **Pakete > nuget-Pakete verwalten... > xamarin. Forms** , und klicken Sie auf **Paket hinzufügen**

1. Erweitern Sie das Erweiterungsprojekt, und ändern Sie einen Einstiegspunkt, um xamarin. Forms zu initialisieren und Seiten zu erstellen. Gemäß den IOS-Anforderungen muss in einer Erweiterung der Einstiegspunkt in " **Info. plist** " als oder definiert werden `NSExtensionMainStoryboard` `NSExtensionPrincipalClass` . Wenn der Einstiegspunkt aktiviert ist, ist es in diesem Fall die `ActionViewController.ViewDidLoad` -Methode. Sie können eine Instanz einer xamarin. Forms-Seite erstellen und für einen Benutzer anzeigen. Öffnen Sie daher den Einstiegspunkt, und ersetzen Sie die- `ViewDidLoad` Methode durch die folgende Implementierung:

    ```csharp
    public override void ViewDidLoad()
    {
        base.ViewDidLoad();

        // Initialize Xamarin.Forms framework
        global::Xamarin.Forms.Forms.Init();
        // Create an instance of XF page with associated View Model
        var xfPage = new MainPage();
        var viewModel = (MainPageViewModel)xfPage.BindingContext;
        viewModel.Message = "Welcome to XF Page created from an iOS Extension";
        // Override the behavior to complete the execution of the Extension when a user press the button
        viewModel.DoCommand = new Command(() => DoneClicked(this));
        // Convert XF page to a native UIViewController which can be consumed by the iOS Extension
        var newController = xfPage.CreateViewController();
        // Present new view controller as a regular view controller
        this.PresentModalViewController(newController, false);
    }
    ```

    Die `MainPage` wird mit einem Standardkonstruktor instanziiert, und bevor Sie Sie in der Erweiterung verwenden können, konvertieren Sie Sie `UIViewController` mithilfe der- `CreateViewController` Erweiterungsmethode in eine native. 
    
    Erstellen Sie die Anwendung und führen Sie Sie aus:

    ![Erweiterung erstellen](extensions-xf-images/3.walkthrough-runapp.png)

    Navigieren Sie zum Aktivieren der Erweiterung zum Safari-Browser, geben Sie eine beliebige Webadresse ein, z. b. [Microsoft.com](https://microsoft.com), klicken Sie auf Navigate, und klicken Sie dann auf das Symbol **Freigeben** unten auf der Seite, um die verfügbaren Aktions Erweiterungen anzuzeigen. Wählen Sie in der Liste der verfügbaren Erweiterungen die Erweiterung **myaction** aus, indem Sie darauf tippen:

    ![Erweiterung erstellen](extensions-xf-images/4.walkthrough-run1.png) ![Erweiterung erstellen](extensions-xf-images/5.walkthrough-run2.png) ![Erweiterung erstellen](extensions-xf-images/6.walkthrough-run3.png)

    Die Erweiterung wird aktiviert, und die xamarin. Forms-Seite wird dem Benutzer angezeigt. Alle Bindungen und Befehle funktionieren wie in der Container-app.

1. Der ursprüngliche Einstiegspunkt-Ansichts Controller ist sichtbar, da er von IOS erstellt und aktiviert wird. Um dieses Problem zu beheben, ändern Sie den modalen Präsentationsstil `UIModalPresentationStyle.FullScreen` für den neuen Controller, indem Sie den folgenden Code direkt vor dem-Befehl hinzufügen `PresentModalViewController` :

    ```csharp
    newController.ModalPresentationStyle = UIModalPresentationStyle.FullScreen;
    ```

    Erstellen und Ausführen im IOS-Simulator oder auf einem Gerät:

    ![Xamarin. Forms in ios-Erweiterung](extensions-xf-images/7.walkthrough-result.png)

    > [!IMPORTANT]
    > Stellen Sie für den gerätebuild sicher, dass Sie die richtigen Buildeinstellungen und die **Releasekonfiguration** verwenden, wie [hier beschrieben](~/iOS/platform/extensions.md#debug-and-release-versions-of-extensions).

## <a name="related-links"></a>Verwandte Links

- [iOS-Erweiterungen in Xamarin.iOS](~/iOS/platform/extensions.md)
- [Xamarin. Forms in nativen xamarin-Projekten](~/xamarin-forms/platform/native-forms.md)
- [Optimieren der Effizienz und Leistung einer IOS-App-Erweiterung](https://developer.apple.com/library/archive/documentation/General/Conceptual/ExtensibilityPG/ExtensionCreation.html#//apple_ref/doc/uid/TP40014214-CH5-SW7)
- [Beispielquellcode](https://github.com/xamcat/xamarin-forms-ios-extension)
