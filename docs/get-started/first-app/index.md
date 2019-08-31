---
title: Erstellen Ihrer ersten Xamarin.Forms-App
description: Videoanleitung zum Erstellen Ihrer ersten Xamarin.Forms-Anwendung in Visual Studio.
zone_pivot_groups: platform-dev16
ms.prod: xamarin
ms.assetid: 72B6AF82-4D98-47E5-AB54-0A35B3253468
ms.technology: xamarin-forms
ms.custom: video
author: conceptdev
ms.author: crdun
ms.date: 05/23/2019
ms.openlocfilehash: 2c50ffb37f0fd1d7b0d9fad063c4d6195d6b1f08
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2019
ms.locfileid: "70199758"
---
# <a name="build-your-first-xamarinforms-app"></a>Erstellen Ihrer ersten Xamarin.Forms-App

_Sehen Sie sich dieses Video an, und gehen Sie wie gezeigt vor, um Ihre erste mobile App mit Xamarin.Forms zu erstellen._

::: zone pivot="windows"

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Build-Your-First-Android-App-with-Visual-Studio-2019-and-Xamarin/player]

## <a name="step-by-step-instructions-for-windows"></a>Exemplarische Vorgehensweise für Windows

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-firstapp/)

Führen Sie die folgenden Schritte zum Video oben aus:

1. Wählen Sie **Datei > neue > Projekt...** aus, oder klicken Sie auf die Schaltfläche **Neues Projekt erstellen...** :

    [![Neues Projekt erstellen](images/win-2019/01-sml.png)](images/win-2019/01.png#lightbox)

2. Suchen Sie nach "xamarin", oder wählen Sie im Menü **Projekttyp** die Option **Mobile** aus. Wählen Sie den Projekttyp **Mobile App (xamarin. Forms)** aus:

    [![Filtern nach xamarin-Projekten](images/win-2019/02-sml.png)](images/win-2019/02.png#lightbox)

3. Wählen Sie einen Projekt &ndash; Namen aus. im Beispiel wird "fantastisch omeapp" verwendet:

    [![Projektnamen auswählen](images/win-2019/03-sml.png)](images/win-2019/03.png#lightbox)

4. Klicken Sie auf den **leeren** Projekttyp, und vergewissern Sie sich, dass **Android** und **IOS** ausgewählt sind:

    [![Android und iOS mit .NET Standard](images/win-2019/04-sml.png)](images/win-2019/04.png#lightbox)

5. Warten Sie, bis die NuGet-Pakete wiederhergestellt sind (wenn die Wiederherstellung abgeschlossen wurde, wird eine Meldung in der Statusleiste angezeigt).

6. In neuen Visual Studio 2019-Installationen ist kein Android-Emulator konfiguriert. Klicken Sie auf den Dropdown Pfeil auf der Schaltfläche " **Debuggen** ", und wählen Sie **Android-Emulator erstellen** aus, um den Bildschirm

    ![Android-Emulator Dropdown Liste erstellen](images/win-2019/debug-dropdown.png)

7. Verwenden Sie auf dem Bildschirm Emulator-Erstellung die Standardeinstellungen, und klicken Sie auf die Schaltfläche **Erstellen** :

    [![Android-Emulator-Erstellungs Bildschirm](images/win-2019/create-emulator-sml.png)](images/win-2019/create-emulator.png#lightbox)

8. Wenn Sie einen Emulator erstellen, werden Sie zum Fenster Geräte-Manager zurückgegeben. Klicken Sie auf die Schaltfläche **starten** , um den neuen Emulator zu starten:

    ![Android-Emulator im Geräte-Manager](images/win-2019/start-emulator.png)

9. Visual Studio 2019 sollte nun den Namen des neuen Emulators auf der Schaltfläche " **Debuggen** " anzeigen:

    ![Android-emulatorname auf der Schaltfläche "Debug"](images/win-2019/debug-emulator-name.png)

10. Klicken Sie auf die Schaltfläche **Debuggen** , um die Anwendung zu erstellen und im Android-Emulator bereitzustellen

    ![Android-Emulator, der die Anwendung anzeigt](images/win-2019/android-emulator.png)

## <a name="customize-the-application"></a>Anpassen der Anwendung

Die Anwendung kann angepasst werden, um interaktive Funktionen hinzuzufügen. Führen Sie die folgenden Schritte aus, um der Anwendung Benutzerinteraktion hinzuzufügen:

1. Bearbeiten Sie die Datei **MainPage.xaml**, und fügen Sie den folgenden XAML-Code vor dem Ende von `</StackLayout>` ein:

    ```xaml
    <Button Text="Click Me" Clicked="Button_Clicked" />
    ```

2. Bearbeiten Sie die Datei **MainPage.xaml.cs**, und fügen Sie den folgenden Code am Ende der Klasse ein:

    ```csharp
    int count = 0;
    void Button_Clicked(object sender, System.EventArgs e)
    {
        count++;
        ((Button)sender).Text = $"You clicked {count} times.";
    }
    ```

3. Debuggen Sie die App unter Android:

    ![Android-App](images/win/07-sml.png)

> [!NOTE]
> Die Beispielanwendung enthält die zusätzlichen interaktiven Funktionen, die im Video nicht behandelt werden.

## <a name="build-an-ios-app-in-visual-studio-2019"></a>Erstellen einer IOS-app in Visual Studio 2019

Es ist möglich, die IOS-App aus Visual Studio mit einem vernetzten Mac-Computer zu erstellen und zu debuggen. Weitere Informationen finden Sie in den [Setupanweisungen](~/ios/get-started/installation/windows/index.md).

In diesem Video wird der Prozess zum entwickeln und Testen einer IOS-App mit Visual Studio 2019 unter Windows behandelt:

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Build-Your-First-iOS-App-with-Visual-Studio-2019-and-Xamarin/player]

::: zone-end
::: zone pivot="win-vs2017"

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Building-Your-First-Android--iOS-App-in-Visual-Studio-2017/player]

## <a name="step-by-step-instructions-for-windows"></a>Exemplarische Vorgehensweise für Windows

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-firstapp/)

Führen Sie die folgenden Schritte zum Video oben aus:

1. Klicken Sie auf **Datei > Neu > Projekt...** , oder klicken Sie auf die Schaltfläche **Neues Projekt erstellen...** , und klicken Sie dann auf **Visual C# > Plattformübergreifend >Mobile App (Xamarin.Forms)** :

    [![Mobile App (Xamarin.Forms)](images/win/01-sml.png)](images/win/01.png#lightbox)

2. Stellen Sie sicher, dass die Plattformen **Android** und **iOS** sowie die **.NET Standard**-Codefreigabe ausgewählt sind:

    [![Android und iOS mit .NET Standard](images/win/02-sml.png)](images/win/02.png#lightbox)

3. Warten Sie, bis die NuGet-Pakete wiederhergestellt sind (wenn die Wiederherstellung abgeschlossen wurde, wird eine Meldung in der Statusleiste angezeigt).

4. Starten Sie den Android-Emulator, indem Sie auf „Debuggen“ klicken (alternativ können Sie auf die Menüelemente **Debuggen > Debuggen starten** klicken).

5. Bearbeiten Sie die Datei **MainPage.xaml**, und fügen Sie den folgenden XAML-Code vor dem Ende von `</StackLayout>` ein:

    ```xaml
    <Button Text="Click Me" Clicked="Button_Clicked" />
    ```

6. Bearbeiten Sie die Datei **MainPage.xaml.cs**, und fügen Sie den folgenden Code am Ende der Klasse ein:

    ```csharp
    int count = 0;
    void Button_Clicked(object sender, System.EventArgs e)
    {
        count++;
        ((Button)sender).Text = $"You clicked {count} times.";
    }
    ```

7. Debuggen Sie die App unter Android:

    ![Android-App](images/win/07-sml.png)

    > [!TIP]
    > Es ist möglich, die iOS-App mit einem Mac-Computer im Netzwerk über Visual Studio zu erstellen und zu debuggen. Weitere Informationen finden Sie in den [Setupanweisungen](~/ios/get-started/installation/windows/index.md).

::: zone-end
::: zone pivot="macos"

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Building-Your-First-iOS--Android-App-in-Visual-Studio-for-Mac/player]

## <a name="step-by-step-instructions-for-mac"></a>Exemplarische Vorgehensweise für Mac

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-firstapp/)

Führen Sie die folgenden Schritte zum Video oben aus:

1. Klicken Sie auf **Datei > Neue Projektmappe...** , oder klicken Sie auf **Neues Projekt...** , und klicken Sie dann auf **Multi-Plattform > App > Leere Forms-App**:

    [![Leere Forms-App](images/01-sml.png)](images/01.png#lightbox)

2. Stellen Sie sicher, dass die Plattformen **Android** und **iOS** sowie die **.NET Standard**-Codefreigabe ausgewählt sind:

    [![Android und iOS mit .NET Standard](images/02-sml.png)](images/02.png#lightbox)

3. Stellen Sie die NuGet-Pakete wieder her, indem Sie mit der rechten Maustaste auf die Projektmappe klicken:

    ![Android-App](images/03-sml.png)

4. Starten Sie den Android-Emulator, indem Sie auf „Debuggen“ klicken (alternativ können Sie auf **Ausführen > Debuggen starten** klicken).

5. Bearbeiten Sie die Datei **MainPage.xaml**, und fügen Sie den folgenden XAML-Code vor dem Ende von `</StackLayout>` ein:

    ```xaml
    <Button Text="Click Me" Clicked="Handle_Clicked" />
    ```

6. Bearbeiten Sie die Datei **MainPage.xaml.cs**, und fügen Sie den folgenden Code am Ende der Klasse ein:

    ```csharp
    int count = 0;
    void Handle_Clicked(object sender, System.EventArgs e)
    {
        count++;
        ((Button)sender).Text = $"You clicked {count} times.";
    }
    ```

7. Debuggen Sie die App unter Android:

    ![Android-App](images/07-sml.png)

8. Klicken Sie mit der rechten Maustaste, und legen Sie iOS als **Startprojekt** fest:

    [![Festlegen von iOS als Startprojekt](images/08-sml.png)](images/08.png#lightbox)

9. Debuggen Sie die App unter iOS:

    ![iOS-App](images/09-sml.png)

::: zone-end

Sie können den vollständigen Code aus dem [Beispielkatalog](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-firstapp/) herunterladen oder auf [GitHub](https://github.com/xamarin/xamarin-forms-samples/tree/master/GetStarted/FirstApp) ansehen.

## <a name="next-steps"></a>Nächste Schritte

- [Single-Page-Schnellstart](~/get-started/quickstarts/single-page.md) &ndash; Erstellen Sie eine funktionstüchtige app.
- [Beispiele für Xamarin.Forms](~/xamarin-forms/samples/index.yml) &ndash; Herunterladen und Ausführen von Codebeispielen und Beispiel-Apps
- [E-Book zum Erstellen von Mobile Apps](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md) &ndash; Diese Buch erläutert die Xamarin.Forms-Entwicklung auf ausführliche Weise. Verfügbar als PDF mit Hunderten zusätzlichen Beispielen.
