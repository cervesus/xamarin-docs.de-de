---
title: Portable Visual Basic.NET
description: Diese Anleitung wird erläutert, wie Visual Basic zum Schreiben von Portable Klassenbibliothek (PCL)-Projekte, die verwendet werden können in Projektmappen für Xamarin.iOS und Xamarin.Android verwendet werden kann.
ms.prod: xamarin
ms.assetid: f264c632-8feb-4015-a5e5-cb9c681c787d
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 71ee153135df97d3b4fa149d3d788d3b940fe944
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
---
# <a name="portable-visual-basicnet"></a>Portable Visual Basic.NET

Xamarin iOS und Android-Projekte unterstützen systemintern Visual Basic; jedoch können Entwickler portablen Klassenbibliotheken zum Migrieren von vorhandenen Visual Basic-Codes für IOS- und Android, oder Schreiben von beträchtlicher Teil der Anwendungslogik in Visual Basic verwenden. Xamarin.Forms-Anwendungen können vollständig in Visual Basic (mit Ausnahme benutzerdefinierter Renderer Abhängigkeitsdienste und Verwendung von XAML-Codebehind) erstellt werden.

## <a name="requirements"></a>Anforderungen

Unterstützung für portable Klassenbibliotheken wurde hinzugefügt, in Xamarin.Android 4.10.1, Xamarin.iOS 7.0.4 und Xamarin Studio 4.2, was bedeutet, dass alle Xamarin-Projekte, die mit diesen Tools erstellte Visual Basic PCL Assemblys integrieren können.

Informationen zum Erstellen und Kompilieren von portablen Klassenbibliotheken in Visual Basic müssen Sie Visual Studio unter Windows (Visual Studio 2012 oder höher) verwenden.

> [!NOTE]
> Visual Basic-Bibliotheken können nur erstellt werden und mithilfe von Visual Studio kompiliert. Xamarin.iOS und Xamarin.Android unterstützt die Sprache Visual Basic nicht.
>
> Wenn Sie ausschließlich in Visual Studio arbeiten, können Sie Visual Basic-Projekt aus Xamarin.iOS und Xamarin.Android Projekten verweisen.
>
> Wenn Ihre IOS- und Android-Projekte auch in Visual Studio für Mac geladen werden müssen sollten Sie die Ausgabeassembly aus der Visual Basic-PCL verweisen.


## <a name="creating-a-visual-basicnet-pcl"></a>Erstellen einer Visual Basic.NET PCL

In diesem Abschnitt erläutert, wie ein Visual Basic Portable Class Library mithilfe von Visual Studio erstellen.
Die Bibliothek kann dann in anderen Projekten, einschließlich Xamarin.Forms, Xamarin.iOS und Xamarin.Android apps verwiesen werden.

### <a name="creating-a-pcl"></a>Erstellen einer PCL

Beim Hinzufügen einer PCL Visual Basic in Visual Studio müssen Sie ein Profil auswählen, die beschreibt, welche Plattformen Ihrer Bibliothek mit kompatibel sein soll. Profile werden in der Einführung zum PCL-Dokument erläutert.

Die Schritte zum Erstellen einer PCL, und wählen ein Profil werden:

1.  In der **neues Projekt** wählen die **Visual Basic > Klassenbibliothek (portabel)** Option:

    [![](images/image1-sml.png "Erstellen Sie neue Portable Visual Basic-Laufzeitbibliothek")](images/image1.png#lightbox)

1.  Visual Studio fordert sofort mit den folgenden **Portable Klassenbibliothek hinzufügen** Dialogfeld, damit das Profil konfiguriert werden kann. Aktivieren Sie die Plattformen müssen Sie unterstützen, und drücken die **OK**.

    [![](images/image2-sml.png "Wählen Sie die PCL-Profil durch Auswählen von Plattformen")](images/image2.png#lightbox)

1.  Visual Basic-PCL-Projekt wird angezeigt, entsprechend der **Projektmappen-Explorer** wie folgt:

    [![](images/image3-sml.png "Leere Visual Studio-PCL-Projekt")](images/image3.png#lightbox)


Die PCL ist nun bereit für Visual Basic-Code hinzugefügt werden. PCL-Projekte können von anderen Projekten (Anwendungsprojekte, Bibliotheksprojekte und auch andere Projekte PCL) verwiesen werden.

### <a name="editing-the-pcl-profile"></a>Bearbeiten das PCL-Profil

Das PCL-Profil (die steuert, welche Plattformen die PCL ist kompatibel mit) angezeigt und geändert werden, indem Sie mit der rechten Maustaste auf das Projekt und können **Eigenschaften > Bibliothek > ändern...** . Das resultierende Dialogfeld ist in diesem Screenshot dargestellt:

 [![](images/image4-sml.png "Bearbeiten Sie in den Projekteigenschaften PCL-Profil")](images/image4.png#lightbox)

Wenn das Profil geändert wird, nachdem der PCL Code bereits hinzugefügt wurde, ist es möglich, dass die Bibliothek nicht mehr kompiliert wird, wenn der Code verweist auf Funktionen, die nicht Teil des neu ausgewählte Profils sind.


## <a name="summary"></a>Zusammenfassung

Dieser Artikel wurde beschrieben, wie Visual Basic-Code in die Xamarin-Anwendungen, die mithilfe von Visual Studio und portablen Klassenbibliotheken genutzt. Obwohl Xamarin Visual Basic nicht direkt unterstützt wird, ermöglicht das Kompilieren von Visual Basic in einer PCL Code mit Visual Basic in iOS und Android-apps aufgenommen werden.

Die folgenden Seiten beschrieben, wie Visual Basic.NET PCLs im einheitlichen Modus oder Xamarin.Forms-apps:

- [Erstellen von systemeigenen apps von Xamarin.iOS und Xamarin.Android, mit denen VB](native-apps.md)
- [Erstellen von Xamarin.Forms-apps mit VB](xamarin-forms.md)


## <a name="related-links"></a>Verwandte Links

- [TaskyPortableVB (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/TaskyPortableVB)
- [XamarinFormsVB (sample)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/XamarinFormsVB)
- [Plattformübergreifende Entwicklung mit .NET Framework (Microsoft)](http://msdn.microsoft.com/library/gg597391(v=vs.110).aspx)
