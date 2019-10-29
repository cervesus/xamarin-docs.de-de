---
title: Xamarin. Forms mithilfe von Visual Basic.net
description: Die xamarin. Forms-Projektvorlage kann so geändert werden, dass Sie Visual Basic für die Hauptassembly verwendet, sodass Sie mit VB.net effektiv plattformübergreifende Mobile Apps erstellen können.
ms.prod: xamarin
ms.assetid: da4b4ba9-9205-47dc-8bae-23272ede2c50
author: davidortinau
ms.author: daortin
ms.date: 04/24/2019
ms.openlocfilehash: e1a540eef2a4d54ead68ae4a9427b0622b668182
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73014551"
---
# <a name="xamarinforms-using-visual-basicnet"></a>Xamarin. Forms mithilfe von Visual Basic.net

Xamarin unterstützt Visual Basic nicht direkt: Befolgen Sie die Anweisungen auf dieser Seite, um C# eine xamarin. Forms-Projekt Mappe zu erstellen C# , und ersetzen Sie dann das .NET Standard Projekt durch Visual Basic.

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/mobile-samples/visualbasic-xamarinformsvb/)

[![erstellen Sie eine xamarin. Forms-Projekt Mappe, und ersetzen Sie dann das .NET Standard Projekt durch Visual Basic](xamarin-forms-images/hero-sml.png)](xamarin-forms-images/hero.png#lightbox)

> [!NOTE]
> Sie müssen Visual Studio unter Windows verwenden, um mit Visual Basic programmieren zu können.

## <a name="xamarinforms-with-visual-basic-walkthrough"></a>Xamarin. Forms mit Visual Basic Exemplarische Vorgehensweise

Führen Sie die folgenden Schritte aus, um ein einfaches xamarin. Forms-Projekt zu erstellen, das Visual Basic verwendet:

1. Wählen Sie in Visual Studio 2019 die Option **Neues Projekt erstellen**aus.

2. Geben Sie im Fenster **Neues Projekt erstellen** **xamarin. Forms** ein, um die Liste zu filtern, und wählen Sie **Mobile App (xamarin. Forms)** aus, und klicken Sie dann auf **weiter**.

    [![Filter für xamarin. Forms-apps](xamarin-forms-images/02-sml.png)](xamarin-forms-images/02.png#lightbox)

3. Geben Sie auf dem nächsten Bildschirm einen Namen für das Projekt ein, und klicken Sie auf **Erstellen**.

4. Wählen Sie die **leere** Vorlage, und klicken Sie auf **OK**:

    [leere xamarin. Forms-Vorlage![](xamarin-forms-images/04-sml.png)](xamarin-forms-images/04.png#lightbox)

    Dadurch wird in Visual Studio mithilfe C#von eine xamarin. Forms-Projekt Mappe erstellt. In den nächsten Schritten wird die Lösung so geändert, dass Sie Visual Basic verwendet.

5. Klicken Sie mit der rechten Maustaste auf die Projekt Mappe, und wählen Sie **> Neues Projekt hinzufügen...**

6. Geben Sie **Visual Basic Bibliothek** ein, um die Projektoptionen zu filtern, und wählen Sie die Option **Klassenbibliothek (.NET Standard)** mit dem Visual Basic Symbol aus:

    [![Filter für Visual Basic Bibliothek](xamarin-forms-images/06-sml.png)](xamarin-forms-images/06.png#lightbox)

7. Geben Sie auf dem nächsten Bildschirm einen Namen für das Projekt ein, und klicken Sie auf **Erstellen**.

8. Klicken Sie mit der rechten Maustaste auf das Projekt Visual Basic, und wählen Sie **Eigenschaften**aus, und ändern Sie dann C# den **Standard Namespace** entsprechend den vorhandenen Projekten:

    [![sicherstellen, dass der Visual Basic Root-Namespace mit der xamarin. Forms-App übereinstimmt](xamarin-forms-images/07a-sml.png)](xamarin-forms-images/07a.png#lightbox)

9. Klicken Sie mit der rechten Maustaste auf das neue Projekt Visual Basic, und wählen Sie **nuget-Pakete verwalten**und dann **xamarin. Forms** installieren aus, und schließen Sie das Fenster Paket-Manager.

    [![Formulare und schließen Sie das Fenster "Paket-Manager"](xamarin-forms-images/07b-sml.png)](xamarin-forms-images/07b.png#lightbox)

10. Benennen Sie die **Class1. vb** -Standarddatei in " **app. vb**" um:

    [![die standardmäßige Class1-Datei und-Klasse in die APP umbenennen](xamarin-forms-images/08.png)](xamarin-forms-images/08.png#lightbox)

11. Fügen Sie den folgenden Code in die Datei " **app. vb** " ein, die als Ausgangspunkt ihrer xamarin. Forms-App verwendet wird:

    ```vb
    Imports Xamarin.Forms

    Public Class App
        Inherits Application

        Public Sub New()
            Dim label = New Label With {.HorizontalTextAlignment = TextAlignment.Center,
                                        .FontSize = Device.GetNamedSize(NamedSize.Medium, GetType(Label)),
                                        .Text = "Welcome to Xamarin.Forms with Visual Basic.NET"}

            Dim stack = New StackLayout With {
                .VerticalOptions = LayoutOptions.Center
            }
            stack.Children.Add(label)

            Dim page = New ContentPage
            page.Content = stack
            MainPage = page

        End Sub

    End Class
    ```

12. Aktualisieren Sie die Android-und IOS-Projekte so, dass Sie auf neue Visual Basic Projekt C# verweisen (und nicht auf das von der Vorlage erstellte Projekt).
Klicken Sie mit der rechten Maustaste auf den Knoten **Verweise** in den Android-und IOS-Projekten, um den **Verweis-Manager**zu öffnen. Deinstallieren Sie die C# Bibliothek, und führen Sie die Visual Basic Bibliothek aus (vergessen Sie nicht, dies für Android-und IOS-Projekte zu tun).

    [![alten Projekt Verweis entfernen, Visual Basic Verweis hinzufügen](xamarin-forms-images/10-sml.png)](xamarin-forms-images/10.png#lightbox)

13. Löschen Sie C# das Projekt. Fügen Sie neue **VB** -Dateien hinzu, um Ihre xamarin. Forms-Anwendung zu erstellen. Eine Vorlage für neue `ContentPage`s in Visual Basic ist unten dargestellt:

    ```vb
    Imports Xamarin.Forms

    Public Class Page2
    Inherits ContentPage

        Public Sub New()
            Dim label = New Label With {.HorizontalTextAlignment = TextAlignment.Center,
                                        .FontSize = Device.GetNamedSize(NamedSize.Medium, GetType(Label)),
                                        .Text = "Visual Basic ContentPage"}

            Dim stack = New StackLayout With {
                .VerticalOptions = LayoutOptions.Center
            }
            stack.Children.Add(label)

            Content = stack
        End Sub
    End Class
    ```

## <a name="limitations-of-visual-basic-in-xamarinforms"></a>Einschränkungen von Visual Basic in xamarin. Forms

Wie auf der [Seite Portable Visual Basic.net](~/cross-platform/platform/visual-basic/index.md)angegeben, unterstützt xamarin die Visual Basic Sprache nicht. Dies bedeutet, dass es einige Einschränkungen gibt, wo Sie Visual Basic verwenden können:

- XAML-Seiten können nicht in das Visual Basic-Projekt eingeschlossen werden, da der Code-Behind C#-Generator nur erstellt werden kann. Sie können XAML in einer separaten, referenzierten, C# portablen Klassenbibliothek einschließen und Datenbindung zum Auffüllen der XAML-Dateien über Visual Basic Modelle verwenden (ein Beispiel hierfür finden Sie im Beispiel) [.](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/XamarinFormsVB/XamlPages)

- Benutzerdefinierte Renderer können nicht in Visual Basic geschrieben werden, sondern müssen in den C# nativen Platt Form Projekten geschrieben werden.

- Abhängigkeits Dienst Implementierungen können nicht in Visual Basic geschrieben werden, sondern müssen in C# den nativen Platt Form Projekten geschrieben werden.

## <a name="related-links"></a>Verwandte Links

- [Xamarinformsvb (Beispiel)](https://docs.microsoft.com/samples/xamarin/mobile-samples/visualbasic-xamarinformsvb/)
- [Plattformübergreifende Entwicklung mit der .NET Framework](https://docs.microsoft.com/dotnet/standard/cross-platform/)
