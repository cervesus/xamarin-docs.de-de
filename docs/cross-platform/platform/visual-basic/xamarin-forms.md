---
title: Xamarin.Forms mithilfe von Visual Basic.NET
description: Xamarin.Forms-PCL-Projektvorlage kann geändert werden, um mithilfe von Visual Basic für die Hauptassembly, sodass Ihnen die Erstellung von plattformübergreifenden mobilen apps, die Visual Basic.NET verwenden.
ms.prod: xamarin
ms.assetid: da4b4ba9-9205-47dc-8bae-23272ede2c50
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: f397cf595a9ae151c5f105341733b2c57023fe99
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61282258"
---
# <a name="xamarinforms-using-visual-basicnet"></a>Xamarin.Forms mithilfe von Visual Basic.NET

Xamarin unterstützt keine Visual Basic direkt – befolgen Sie die Anweisungen auf dieser Seite erstellen Sie eine C#-Xamarin.Forms-PCL-Projektmappe, und Ersetzen Sie den allgemeinen Code PCL-Projekt mit Visual Basic.

[![](xamarin-forms-images/hero-sml.png "Erstellen Sie eine Xamarin.Forms-PCL-Projektmappe, und Ersetzen Sie den allgemeinen Code PCL-Projekt mit Visual Basic")](xamarin-forms-images/hero.png#lightbox)

> [!NOTE]
> Sie müssen Visual Studio auf Windows Programmierung mit Visual Basic verwenden.

## <a name="xamarinforms-with-visual-basic-walkthrough"></a>Xamarin.Forms mit Visual Basic Exemplarische Vorgehensweise

Um eine einfache Xamarin.Forms-Projekt zu erstellen, das Visual Basic verwendet, gehen Sie wie folgt vor:

1. Erstellen Sie ein neues *Xamarin.Forms c#* Lösung, Portable Class Libraries (PCL) verwendet.
Wechseln Sie zu **Datei > Neues Projekt** und klicken Sie in der **neues Projekt** Fenster Navigieren Sie zu **installiert > Vorlagen > Visual C# > plattformübergreifend** wählen Sie dann auf **Cross Platform App (Xamarin.Forms oder nativ) > Xamarin.Forms**.

2. Mit der rechten Maustaste auf die Projektmappe und **hinzufügen > Neues Projekt**.

3. Wählen Sie die **Visual Basic > Klassenbibliothek (portabel)** Projekttyp:

   [![](xamarin-forms-images/add-vb-2-sml.png "Fügen Sie neue portable Class Library-Projekt hinzu.")](xamarin-forms-images/add-vb-2.png#lightbox)

4. Wählen Sie die Plattformen aus, wie gezeigt, um das richtige PCL-Profil konfigurieren (Achten Sie darauf, Xamarin.iOS und Xamarin.Android enthalten):

   ![](xamarin-forms-images/add-vb-3-sml.png "Wählen Sie die Plattformen unterstützen")

5. Mit der rechten Maustaste auf die Visual Basic-Projekt, und wählen Sie **Eigenschaften**, ändern Sie dann die **Standardnamespace** entsprechend der vorhandenen C#-Projekte:

   ![](xamarin-forms-images/add-vb-4s-sml.png "Stellen Sie sicher, dass die Visual Basic-Stamm-Namespace die Xamarin.Forms-app entspricht")

6. Mit der rechten Maustaste auf das neue Visual Basic-Projekt, und wählen Sie **Nuget-Pakete verwalten**, installieren Sie dann **Xamarin.Forms** , und schließen Sie das Fenster der Paket-Manager.

   [![](xamarin-forms-images/add-vb-4-sml.png "Formulare und schließen Sie das Fenster der Paket-manager")](xamarin-forms-images/add-vb-4.png#lightbox)

7. Benennen Sie das standardmäßige **Class1** Datei *und* -Klasse `App`:

   [![](xamarin-forms-images/add-vb-5-sml.png "Benennen Sie die Class1-Standarddatei und die Klasse für App")](xamarin-forms-images/add-vb-5.png#lightbox)

8. Fügen Sie den folgenden Code in die **App.vb** -Datei, die den Anfangspunkt der Xamarin.Forms-app werden soll. Denken Sie daran, `Imports Xamarin.Forms` und fügen `Inherits Application` der Klasse:

    ```vb 
    Imports Xamarin.Forms

    Public Class App
        Inherits Application

        Public Sub New()
            Dim label = New Label With {.HoriztonalTextAlignment = TextAlignment.Center,
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

9. Nun müssen wir die IOS- und Android-Projekte auf das neue Visual Basic-Projekt zu verweisen.
Mit der rechten Maustaste auf die **Verweise** Knoten in den IOS- und Android-Projekte zum Öffnen der **Verweis-Manager**. Un-Tick der C# portable Bibliotheken und -Tick der portablen VB--Bibliothek (durch nicht vergessen, dies gilt für IOS- und Android-Projekte).

   [![](xamarin-forms-images/add-vb-8-sml.png "Entfernen der alten Projektverweis, Hinzufügen von Visual Basic-Referenz")](xamarin-forms-images/add-vb-8.png#lightbox)

10. Löschen Sie portable c#-Projekt. Hinzufügen neuer **vb** zu erstellende out Ihrer Xamarin.Forms-Anwendung. Eine Vorlage für neue `ContentPage`"s" in Visual Basic wird im folgenden dargestellt:

    ```vb
    Imports Xamarin.Forms

    Public Class Page2
    Inherits ContentPage

        Public Sub New()
            Dim label = New Label With {.HoriztonalTextAlignment = TextAlignment.Center,
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

## <a name="limitations-of-visual-basic-in-xamarinforms"></a>Einschränkungen von Visual Basic in Xamarin.Forms

Wie erwähnt in der [Portable Visual Basic.NET Seite](~/cross-platform/platform/visual-basic/index.md), Xamarin Visual Basic-Sprache nicht unterstützt. Dies bedeutet, dass es einige Einschränkungen auf, in denen Sie Visual Basic verwenden können:

 - Benutzerdefinierte Renderer in Visual Basic geschrieben werden können, müssen sie in c# in den Projekten der nativen Plattform geschrieben werden.

 - Abhängigkeit dienstimplementierungen in Visual Basic geschrieben werden können, müssen sie in c# in den Projekten der nativen Plattform geschrieben werden.

 - XAML-Seiten nicht in Visual Basic-Projekt enthalten sein: den Code-Behind-Generator kann nur erstellt C# -Code. Es ist möglich, die XAML in eine separate, auf die verwiesen wird, C#-Klassenbibliothek und Datenbindung zum Auffüllen der XAML-Dateien über Visual Basic-Modelle (ein Beispiel hierfür befindet sich auf die [Beispiel](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/XamarinFormsVB/XamlPages)).

 - Xamarin unterstützt nicht die Sprache Visual Basic.NET.

## <a name="related-links"></a>Verwandte Links

- [XamarinFormsVB (sample)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/XamarinFormsVB)
- [Plattformübergreifende Entwicklung mit .NET Framework](https://docs.microsoft.com/dotnet/standard/cross-platform/)
