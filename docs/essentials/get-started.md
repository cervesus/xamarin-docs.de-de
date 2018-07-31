---
title: Erste Schritte mit Xamarin.Essentials
description: Xamarin.Essentials stellt eine einzelnen plattformübergreifende API, die mit jeder iOS-, Android- oder UWP-Anwendung aus zugegriffen werden kann, die freigegebenen Code unabhängig davon, wie die Benutzeroberfläche erstellt wird.
ms.assetid: B2669C48-B659-4854-BD80-FEB0E876F5B9
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: c72c1c66a465075770ce739270cb4b1f2c6fba7a
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353775"
---
# <a name="get-started-with-xamarinessentials"></a>Erste Schritte mit Xamarin.Essentials

![Vorabversionen von NuGet](~/media/shared/pre-release.png)

Xamarin.Essentials stellt eine einzelnen plattformübergreifende API, die mit jeder iOS-, Android- oder UWP-Anwendung aus zugegriffen werden kann, die freigegebenen Code unabhängig davon, wie die Benutzeroberfläche erstellt wird.

## <a name="platform-support"></a>Plattformunterstützung

Xamarin.Essentials unterstützt die folgenden Betriebssysteme und Plattformen:

| Plattform | Version |
| --- | --- |
| Android | 4.4 (API-19) oder höher |
| iOS |10.0 oder höher |
| UWP | 10.0.16299.0 oder höher |

## <a name="installation"></a>Installation

Xamarin.Essentials steht als NuGet-Paket, das alle vorhandenen oder neuen Projekt, die mithilfe von Visual Studio hinzugefügt werden kann.

1. Herunterladen und installieren [Visual Studio](http://visualstudio.com) mit der [Visual Studio-Tools für Xamarin](~/cross-platform/get-started/installation/index.md).

2. Öffnen Sie ein vorhandenes Projekt, oder erstellen Sie ein neues Projekt mithilfe der Vorlage der leeren App unter **Visual Studio C#-** (Android, iPhone und iPad oder Cross-Platform). **Wichtige**: Wenn hinzufügen zu einem UWP-Projekt sicherstellen Build 16299 oder höher in den Projekteigenschaften festgelegt ist.

3. Hinzufügen der **Xamarin.Essentials** NuGet-Paket jedem Projekt:

    # <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

    Klicken Sie im Bereich Projektmappen-Explorer klicken Sie mit der rechten Maustaste auf den Namen der Projektmappe, und wählen Sie **NuGet-Pakete verwalten**. Suchen Sie nach **Xamarin.Essentials** und installieren Sie das Paket in **alle** Projekte, einschließlich Android, iOS, UWP und .NET Standard-Bibliotheken.

    > [!TIP]
    > Überprüfen Sie die **Vorabversion einbeziehen** Feld während der [ **Xamarin.Essentials** NuGet](https://www.nuget.org/packages/Xamarin.Essentials) ist als Vorschauversion verfügbar.

    # <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

    Klicken Sie im Bereich Projektmappen-Explorer klicken Sie mit der rechten Maustaste auf den Projektnamen, und wählen **hinzufügen > NuGet-Pakete hinzufügen...** . Suchen Sie nach **Xamarin.Essentials** und installieren Sie das Paket in **alle** -Projekten, einschließlich Android, iOS und .NET Standard-Bibliotheken.

    > [!TIP]
    > Überprüfen Sie die **vorabversionspakete anzeigen** Feld während der [ **Xamarin.Essentials** NuGet](https://www.nuget.org/packages/Xamarin.Essentials) ist als Vorschauversion verfügbar.

    -----

4. Fügen Sie einen Verweis auf Xamarin.Essentials, in einer C#-Klasse auf die APIs verweisen.

    ```csharp
    using Xamarin.Essentials;
    ```

5. Xamarin.Essentials ist erforderlich, plattformspezifischen Setup:

    # <a name="androidtabandroid"></a>[Android](#tab/android)

    Xamarin.Essentials unterstützt mindestens Android 4.4, API-Ebene 19, für die Version der Android-Zielversion für die Kompilierung muss jedoch 8.1, API-Ebene 27 entspricht. (In Visual Studio werden diese beiden Versionen im Dialogfeld "Projekteigenschaften" für das Android-Projekt, in der Registerkarte "Android-Manifest" festgelegt. In Visual Studio für Mac sind sie in das Dialogfeld "Projektoptionen" für das Android-Projekt, in der Registerkarte "Android-Anwendung" festgelegt.) 
    
    Xamarin.Essentials installiert Version 27.0.2.1 der Xamarin.Android.Support-Bibliotheken, die es benötigt. Alle anderen Xamarin.Android.Support-Bibliotheken, die Ihre Anwendung erfordert sollte ebenfalls auf Version 27.0.2.1 mithilfe des NuGet-Paket-Managers aktualisiert werden. Alle Xamarin.Android.Support-Bibliotheken, die von Ihrer Anwendung verwendeten sollte identisch sein und muss mindestens Version 27.0.2.1. Finden Sie in der [Problembehandlungsseite](troubleshooting.md) Wenn Sie Probleme, NuGet-Xamarin.Essentials hinzufügen oder Aktualisieren von NuGet-Pakete in der Projektmappe haben.

    In der Android-Projekt `MainLauncher` oder ein beliebiges `Activity` , gestarteten Xamarin.Essentials muss initialisiert werden, der `OnCreate` Methode:

    ```csharp
    Xamarin.Essentials.Platform.Init(this, bundle);
    ```

    Behandeln Sie Laufzeitberechtigungen für Android müssen Xamarin.Essentials erhalten alle `OnRequestPermissionsResult`. Fügen Sie den folgenden Code auf alle `Activity` Klassen:

    ```csharp
    public override void OnRequestPermissionsResult(int requestCode, string[] permissions, [GeneratedEnum] Android.Content.PM.Permission[] grantResults)
    {
        Xamarin.Essentials.Platform.OnRequestPermissionsResult(requestCode, permissions, grantResults);

        base.OnRequestPermissionsResult(requestCode, permissions, grantResults);
    }
    ```

    # <a name="iostabios"></a>[iOS](#tab/ios)

    Ohne zusätzliche Einrichtung erforderlich.

    # <a name="uwptabuwp"></a>[UWP](#tab/uwp)

    Ohne zusätzliche Einrichtung erforderlich.

    -----

6. Führen Sie die [Xamarin.Essentials Handbücher](index.md) , mit denen Sie zum Kopieren und Einfügen von Codeausschnitten für die einzelnen Funktionen.

## <a name="other-resources"></a>Weitere Ressourcen

Wir empfehlen Entwicklern, die noch nicht mit Xamarin finden Sie unter [erste Schritte mit Xamarin-Entwicklung](~/cross-platform/getting-started/index.md).

Besuchen Sie die [Xamarin.Essentials GitHub-Repository](http://github.com/xamarin/Essentials) , finden Sie unter dem aktuellen Quellcode, was als Nächstes ansteht Ausführen von Beispielen, und schließen Sie das Repository. Community-Beiträge sind Willkommen!

Navigieren Sie durch die [-API-Dokumentation](xref:Xamarin.Essentials) für sämtliche Features Xamarin.Essentials.
