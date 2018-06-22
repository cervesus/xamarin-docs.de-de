---
title: Xamarin.Forms mit Visual Basic.NET
description: Xamarin.Forms PCL-Projektvorlage kann geändert werden, um die Visual Basic für die Hauptassembly, ermöglicht Ihnen das Erstellen von plattformübergreifenden mobilen apps mithilfe von VB.NET effektiv verwenden.
ms.prod: xamarin
ms.assetid: da4b4ba9-9205-47dc-8bae-23272ede2c50
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: b858e26de95d2abbc23917b1ed5a1de65105cd8d
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
ms.locfileid: "33917862"
---
# <a name="xamarinforms-using-visual-basicnet"></a>Xamarin.Forms mit Visual Basic.NET

Xamarin unterstützt keine Visual Basic direkt – befolgen Sie die Anweisungen auf dieser Seite können Sie eine C#-Xamarin.Forms PCL-Projektmappe erstellen, und Ersetzen Sie den gemeinsame Code PCL-Projekt mit Visual Basic.

[![](xamarin-forms-images/hero-sml.png "Erstellen Sie eine Xamarin.Forms PCL-Projektmappe, und Ersetzen Sie den gemeinsame Code PCL-Projekt mit Visual Basic")](xamarin-forms-images/hero.png#lightbox)

> [!NOTE]
> Sie müssen Visual Studio unter Windows Programmierung mit Visual Basic verwenden.

## <a name="xamarinforms-with-visual-basic-walkthrough"></a>Xamarin.Forms mit Visual Basic exemplarischen Vorgehensweise

Führen Sie die folgenden Schritte aus, um ein einfaches Xamarin.Forms-Projekt zu erstellen, das Visual Basic verwendet:

1. Erstellen Sie ein neues *Xamarin.Forms c#* Lösung, die portablen Klasse Bibliotheken (PCL) verwendet.
Wechseln Sie zu **Datei > Neues Projekt** und klicken Sie in der **neues Projekt** Fenster, navigieren Sie zu **installiert > Vorlagen > Visual c# > plattformübergreifend** wählen Sie dann  **Cross-Plattform-App (Xamarin.Forms oder systemeigen) > Xamarin.Forms**.

2. Mit der rechten Maustaste auf die Projektmappe und **hinzufügen > Neues Projekt**.

3. Wählen Sie die **Visual Basic > Klassenbibliothek (portabel)** Projekttyp:

   [![](xamarin-forms-images/add-vb-2-sml.png "Fügen Sie neue portable Klassenbibliotheksprojekt hinzu.")](xamarin-forms-images/add-vb-2.png#lightbox)

4. Wählen Sie die Plattformen, wie dargestellt, um das richtige PCL-Profil konfigurieren (achten Xamarin.iOS und Xamarin.Android):

   ![](xamarin-forms-images/add-vb-3-sml.png "Wählen Sie die Plattformen zu unterstützen")

5. Mit der rechten Maustaste auf das Visual Basic-Projekt, und wählen Sie **Eigenschaften**, ändern Sie dann die **Standardnamespace** entsprechend der vorhandenen C#-Projekte:

   ![](xamarin-forms-images/add-vb-4s-sml.png "Sicherzustellen Sie, dass der Visual Basic-Stammnamespace Xamarin.Forms app übereinstimmt")

6. Mit der rechten Maustaste auf das neue Visual Basic-Projekt, und wählen Sie **NuGet-Pakete verwalten**, befestigen Sie **Xamarin.Forms** , und schließen Sie das Fenster "Paket-Manager".

   [![](xamarin-forms-images/add-vb-4-sml.png "Formulare und schließen Sie das Paket-Manager-Fenster")](xamarin-forms-images/add-vb-4.png#lightbox)

7. Der standardmäßigen **Class1** Datei *und* Klasse `App`:

   [![](xamarin-forms-images/add-vb-5-sml.png "Benennen Sie die Class1-Standarddatei und die Klasse in der App")](xamarin-forms-images/add-vb-5.png#lightbox)

8. Fügen Sie den folgenden Code in die **App.vb** -Datei, die den Anfangspunkt der Ihrer app Xamarin.Forms werden soll. Denken Sie daran, `Imports Xamarin.Forms` und hinzufügen `Inherits Application` der Klasse:

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

9. Nun müssen wir IOS- und Android-Projekte auf das neue Visual Basic-Projekt verweisen.
Mit der rechten Maustaste auf die **Verweise** Knoten in der IOS- und Android-Projekte zum Öffnen der **Verweis-Manager**. Un-Takt der C#-portable Bibliothek und Tick der portablen VB-Bibliothek (Sie nicht vergessen, auf keinen Fall für IOS- und Android-Projekte).

   [![](xamarin-forms-images/add-vb-8-sml.png "Entfernen Sie alte Projektverweis, Hinzufügen von Visual Basic-Referenz")](xamarin-forms-images/add-vb-8.png#lightbox)

10. Löschen Sie das portable C#-Projekt. Hinzufügen neuer **vb** out-Dateien zum Erstellen Ihrer Anwendung Xamarin.Forms. Eine Vorlage für neue `ContentPage`s in Visual Basic wird unten gezeigt:

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

Wie auf der [Portable Visual Basic.NET Seite](~/cross-platform/platform/visual-basic/index.md), Xamarin die Sprache Visual Basic nicht unterstützt. Dies bedeutet, dass es gibt einige Einschränkungen auf, in denen Sie Visual Basic verwenden können:

 - Benutzerdefinierte Renderern in Visual Basic geschrieben werden können, müssen sie in c# in die eigene Plattformprojekte geschrieben werden.

 - Abhängigkeit dienstimplementierungen in Visual Basic geschrieben werden können, müssen sie in c# in der systemeigenen Plattformprojekte geschrieben werden.

 - XAML-Seiten nicht im Visual Basic-Projekt enthalten sein: der Code-Behind-Generator kann nur erstellt c#. Es ist möglich, die XAML in eine separate, auf die verwiesen wird, C#-portable Klassenbibliothek einschließen und Databinding zum Auffüllen der XAML-Dateien über Visual Basic-Modelle verwenden (ein Beispiel hierfür ist in enthalten die [Beispiel](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/XamarinFormsVB/XamlPages)).

 - Die Sprache Visual Basic.NET unterstützt Xamarin nicht.

## <a name="related-links"></a>Verwandte Links

- [XamarinFormsVB (sample)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/XamarinFormsVB)
- [Plattformübergreifende Entwicklung mit .NET Framework (Microsoft)](http://msdn.microsoft.com/en-us/library/gg597391(v=vs.110).aspx)
