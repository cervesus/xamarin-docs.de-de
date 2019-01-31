---
title: Erstellen Ihrer ersten Xamarin.Forms-App
description: Videoanleitung zum Erstellen Ihrer ersten Xamarin.Forms-Anwendung in Visual Studio.
zone_pivot_groups: platform
ms.prod: xamarin
ms.assetid: 72B6AF82-4D98-47E5-AB54-0A35B3253468
ms.technology: xamarin-forms
ms.custom: video
author: conceptdev
ms.author: crdun
ms.date: 09/24/2018
ms.openlocfilehash: d89488e3f6e42f84fc9519eedaa38c99b90ae068
ms.sourcegitcommit: 817d26585093cd180a36b28179eb354b0eb900b3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55293047"
---
# <a name="build-your-first-xamarinforms-app"></a>Erstellen Ihrer ersten Xamarin.Forms-App

_Sehen Sie sich dieses Video an, und gehen Sie wie gezeigt vor, um Ihre erste mobile App mit Xamarin.Forms zu erstellen._

::: zone pivot="windows"

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Building-Your-First-Android--iOS-App-in-Visual-Studio-2017/player]

## <a name="step-by-step-instructions-for-windows"></a>Exemplarische Vorgehensweise für Windows

Führen Sie die folgenden Schritte zum Video oben aus:

1. Klicken Sie auf **Datei > Neu > Projekt...**, oder klicken Sie auf die Schaltfläche **Neues Projekt erstellen...**, und klicken Sie dann auf **Visual C# > Plattformübergreifend >Mobile App (Xamarin.Forms)**:

    [![Mobile App (Xamarin.Forms)](images/win/01-sml.png)](images/win/01.png#lightbox)

2. Stellen Sie sicher, dass die Plattformen **Android** und **iOS** sowie die **.NET Standard**-Codefreigabe ausgewählt sind:

    [![Android und iOS mit .NET Standard](images/win/02-sml.png)](images/win/02.png#lightbox)

3. Warten Sie, bis die NuGet-Pakete wiederhergestellt sind (wenn die Wiederherstellung abgeschlossen wurde, wird eine Meldung in der Statusleiste angezeigt).

4. Starten Sie den Android-Emulator, indem Sie auf „Debuggen“ klicken (alternativ können Sie auf die Menüelemente **Debuggen > Debuggen starten** klicken).

5. Bearbeiten Sie die Datei **MainPage.xaml**, und fügen Sie den folgenden XAML-Code vor dem Ende von `</StackPanel>` ein:

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

Führen Sie die folgenden Schritte zum Video oben aus:

1. Klicken Sie auf **Datei > Neue Projektmappe...**, oder klicken Sie auf **Neues Projekt...**, und klicken Sie dann auf **Multi-Plattform > App > Leere Forms-App**:

    [![Leere Forms-App](images/01-sml.png)](images/01.png#lightbox)

2. Stellen Sie sicher, dass die Plattformen **Android** und **iOS** sowie die **.NET Standard**-Codefreigabe ausgewählt sind:

    [![Android und iOS mit .NET Standard](images/02-sml.png)](images/02.png#lightbox)

3. Stellen Sie die NuGet-Pakete wieder her, indem Sie mit der rechten Maustaste auf die Projektmappe klicken:

    ![Android-App](images/03-sml.png)

4. Starten Sie den Android-Emulator, indem Sie auf „Debuggen“ klicken (alternativ können Sie auf **Ausführen > Debuggen starten** klicken).

5. Bearbeiten Sie die Datei **MainPage.xaml**, und fügen Sie den folgenden XAML-Code vor dem Ende von `</StackPanel>` ein:

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

Sie können den vollständigen Code aus dem [Beispielkatalog](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/FirstApp/) herunterladen oder auf [GitHub](https://github.com/xamarin/xamarin-forms-samples/tree/master/GetStarted/FirstApp) ansehen.

## <a name="next-steps"></a>Nächste Schritte

- [Einzelne Page Quickstart](~/get-started/quickstarts/single-page.md) &ndash; erstellen Sie eine app mehr funktionelle.
- [Beispiele für Xamarin.Forms](~/xamarin-forms/samples/index.yml) &ndash; Herunterladen und Ausführen von Codebeispielen und Beispiel-Apps
- [E-Book zum Erstellen von Mobile Apps](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md) &ndash; Diese Buch erläutert die Xamarin.Forms-Entwicklung auf ausführliche Weise. Verfügbar als PDF mit Hunderten zusätzlichen Beispielen.
