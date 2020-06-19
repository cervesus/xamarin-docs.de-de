---
title: 'title: "Erstellen Ihrer ersten Xamarin.Forms-App" description: "Dies ist eine Videoanleitung zum Erstellen Ihrer ersten Xamarin.Forms-Anwendung in Visual Studio."'
description: 'zone_pivot_groups: platform-dev16 ms.prod: xamarin ms.assetid: 72B6AF82-4D98-47E5-AB54-0A35B3253468 ms.technology: xamarin-forms ms.custom: video author: conceptdev ms.author: crdun ms.date: 05/23/2019 no-loc: [Xamarin.Forms, Xamarin.Essentials]'
zone_pivot_groups: platform-dev16
ms.prod: xamarin
ms.assetid: 72B6AF82-4D98-47E5-AB54-0A35B3253468
ms.technology: xamarin-forms
ms.custom: video
author: conceptdev
ms.author: crdun
ms.date: 05/23/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: da56bde956a0ff7730ef6737e2802c3723d6d716
ms.sourcegitcommit: ea9269b5d9e3d68b61bb428560a10034117ee457
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2020
ms.locfileid: "84133474"
---
# <a name="build-your-first-xamarinforms-app"></a>Erstellen Ihrer ersten Xamarin.Forms-App

_Sehen Sie sich dieses Video an, und gehen Sie wie gezeigt vor, um Ihre erste mobile App mit Xamarin.Forms zu erstellen._

::: zone pivot="windows"

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Build-Your-First-Android-App-with-Visual-Studio-2019-and-Xamarin/player]

## <a name="step-by-step-instructions-for-windows"></a>Exemplarische Vorgehensweise für Windows

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-firstapp/)

Führen Sie die folgenden Schritte zum Video oben aus:

1. Klicken Sie auf **Datei > Neu > Projekt…** , oder klicken Sie auf die Schaltfläche **Neues Projekt erstellen…** :

    [![Neues Projekt erstellen](images/win-2019/01-sml.png)](images/win-2019/01.png#lightbox)

2. Suchen Sie nach „Xamarin“, oder wählen Sie im Menü **Projekttyp** die Option **Mobil** aus. Wählen Sie den Projekttyp **Mobile App (Xamarin.Forms)** aus:

    [![Filtern nach Xamarin-Projekten](images/win-2019/02-sml.png)](images/win-2019/02.png#lightbox)

3. Wählen Sie einen Projektnamen &ndash; im Beispiel wird der Name „AwesomeApp“ verwendet:

    [![Eingeben eines Projektnamens](images/win-2019/03-sml.png)](images/win-2019/03.png#lightbox)

4. Klicken Sie auf den Projekttyp **Leer**, und vergewissern Sie sich, dass **Android** und **iOS** ausgewählt sind:

    [![Android und iOS mit .NET Standard](images/win-2019/04-sml.png)](images/win-2019/04.png#lightbox)

5. Warten Sie, bis die NuGet-Pakete wiederhergestellt sind (wenn die Wiederherstellung abgeschlossen wurde, wird eine Meldung in der Statusleiste angezeigt).

6. Für neue Visual Studio 2019-Installationen wird kein Android-Emulator konfiguriert. Klicken Sie auf die Dropdownschaltfläche auf der Schaltfläche **Debuggen**, und wählen Sie dort die Option **Android-Emulator erstellen…** aus, um den Bildschirm zum Erstellen des Emulator aufzurufen:

    ![Dropdownmenü „Android Emulator erstellen…“](images/win-2019/debug-dropdown.png)

7. Verwenden Sie im Bildschirm zum Erstellen des Emulators die Standardeinstellungen, und klicken Sie auf die Schaltfläche **Erstellen**:

    [![Bildschirm zum Erstellen des Android-Emulators](images/win-2019/create-emulator-sml.png)](images/win-2019/create-emulator.png#lightbox)

8. Das Erstellen eines Emulators führt Sie zurück zum Geräte-Manager-Fenster. Klicken Sie auf die Schaltfläche **Starten**, damit der neue Emulator gestartet wird:

    ![Android-Emulator im Geräte-Manager](images/win-2019/start-emulator.png)

9. In Visual Studio 2019 sollte nun der Name des neuen Emulators auf der Schaltfläche **Debuggen** angezeigt werden.

    ![Name des Android-Emulators auf der Schaltfläche „Debuggen“](images/win-2019/debug-emulator-name.png)

10. Klicken Sie auf die Schaltfläche **Debuggen**, damit die Anwendung erstellt und für den Android-Emulator bereitgestellt wird:

    ![Der Android-Emulator zeigt die Anwendung an](images/win-2019/android-emulator.png)

## <a name="customize-the-application"></a>Anpassen der Anwendung

Die Anwendung kann angepasst werden, um interaktive Funktionen hinzuzufügen. Führen Sie die folgenden Schritte aus, um der Anwendung die Möglichkeit zur Benutzerinteraktion hinzuzufügen:

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
> In der Beispielanwendung ist die zusätzliche interaktive Funktionalität enthalten, die im Video nicht thematisiert wird.

## <a name="build-an-ios-app-in-visual-studio-2019"></a>Erstellen einer iOS-App in Visual Studio 2019

Es ist möglich, die iOS-App mit einem Mac-Computer im Netzwerk über Visual Studio zu erstellen und zu debuggen. Weitere Informationen finden Sie in den [Setupanweisungen](~/ios/get-started/installation/windows/index.md).

In diesem Video erhalten Sie Informationen zum Erstell- und Testprozess einer iOS-App mithilfe von Visual Studio 2019 unter Windows:

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Build-Your-First-iOS-App-with-Visual-Studio-2019-and-Xamarin/player]

::: zone-end
::: zone pivot="win-vs2017"

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Building-Your-First-Android--iOS-App-in-Visual-Studio-2017/player]

## <a name="step-by-step-instructions-for-windows"></a>Exemplarische Vorgehensweise für Windows

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-firstapp/)

Führen Sie die folgenden Schritte zum Video oben aus:

1. Klicken Sie auf **Datei > Neu > Projekt...** , oder klicken Sie auf die Schaltfläche **Neues Projekt erstellen...** und dann auf **Visual C# > Plattformübergreifend > Mobile App (Xamarin.Forms)** :

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

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-firstapp/)

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

- [Erstellen einer Single-Page-Anwendung mit Xamarin.Forms](~/get-started/quickstarts/single-page.md) &ndash; In diesem Artikel erfahren Sie, wie Sie eine funktionalere App erstellen.
- [Xamarin.Forms-Beispiele](~/xamarin-forms/samples/index.md) &ndash; In diesem Artikel können Sie Codebeispiele und Beispiel-Apps herunterladen und ausführen.
- [E-Book über das Erstellen mobiler Apps](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md) &ndash; In diesem Buch finden Sie detaillierte Kapitel, in denen Sie das Entwickeln mit Xamarin.Forms erlernen können. Die Kapitel sind als PDF verfügbar, und es sind Hunderte zusätzliche Beispiele enthalten.
