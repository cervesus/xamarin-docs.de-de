---
title: Portable Visual Basic.NET
description: Dieses Handbuch wird erläutert, wie Visual Basic zum Schreiben von portablen Klassenbibliothek (PCL)-Projekte, die verwendet werden können in Lösungen, die für Xamarin.iOS und Xamarin.Android verwendet werden kann.
ms.prod: xamarin
ms.assetid: f264c632-8feb-4015-a5e5-cb9c681c787d
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: e4c8c43b4df1a7bfc5436f14564c6d0164216c46
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2019
ms.locfileid: "57669244"
---
# <a name="portable-visual-basicnet"></a>Portable Visual Basic.NET

Xamarin.IOS und Android-Projekte unterstützen nicht systemintern Visual Basic. jedoch können Entwickler portabler Klassenbibliotheken zum Migrieren von vorhandenen Visual Basic-Codes für iOS und Android, oder Schreiben von großen Teil der Anwendungslogik in Visual Basic verwenden. Xamarin.Forms-Anwendungen können vollständig in Visual Basic (mit Ausnahme von benutzerdefinierten Renderern und abhängigkeitsdiensten XAML-Codebehind) erstellt werden.

## <a name="requirements"></a>Anforderungen

Unterstützung für portable Klassenbibliotheken wurde hinzugefügt, in Xamarin.Android 4.10.1, Xamarin.iOS 7.0.4 und Xamarin Studio 4.2, was bedeutet, dass alle Xamarin-Projekte, die mit diesen Tools erstellte Visual Basic-PCL-Assemblys integrieren können.

Zum Erstellen und Kompilieren von portablen Klassenbibliotheken in Visual Basic müssen Sie Visual Studio für Windows (Visual Studio 2012 oder höher) verwenden.

> [!NOTE]
> Visual Basic-Bibliotheken können nur erstellt werden und mithilfe von Visual Studio kompiliert. Xamarin.iOS und Xamarin.Android unterstützen nicht die Visual Basic-Sprache.
>
> Wenn Sie ausschließlich in Visual Studio arbeiten, können Sie Visual Basic-Projekt von Xamarin.iOS und Xamarin.Android-Projekte verweisen.
>
> Wenn IOS- und Android-Projekte auch in Visual Studio für Mac geladen werden müssen, sollten Sie die Ausgabeassembly aus der Visual Basic-PCL verweisen.


## <a name="creating-a-visual-basicnet-pcl"></a>Erstellen eine PCL für Visual Basic.NET

In diesem Abschnitt erläutert, wie ein Visual Basic Portable Class Library mithilfe von Visual Studio zu erstellen.
Die Bibliothek kann dann in anderen Projekten, einschließlich Xamarin.iOS, Xamarin.Android und Xamarin.Forms-apps verwiesen werden.

### <a name="creating-a-pcl"></a>Erstellen eine PCL

Beim Hinzufügen einer PCL Visual Basic in Visual Studio müssen Sie ein Profil auswählen, die beschreibt, welche Plattformen Ihrer Bibliothek mit kompatibel sein soll. Profile werden in der Einführung zu PCL-Dokument erläutert.

Die Schritte zum Erstellen einer PCL, und wählen das Profil sind:

1.  In der **neues Projekt** auf die **Visual Basic > Klassenbibliothek (portabel)** Option:

    [![](images/image1-sml.png "Erstellen Sie neue Portable Visual Basic-Bibliothek")](images/image1.png#lightbox)

1.  Visual Studio fordert sofort durch den folgenden **Portable Klassenbibliothek hinzufügen** Dialogfeld, damit das Profil konfiguriert werden kann. Aktivieren Sie die Plattformen zu unterstützen, und drücken die benötigten **OK**.

    [![](images/image2-sml.png "Wählen Sie PCL-Profil durch Auswählen der Plattformen")](images/image2.png#lightbox)

1.  Visual Basic-PCL-Projekt wird angezeigt, siehe die **Projektmappen-Explorer** wie folgt aus:

    [![](images/image3-sml.png "Leere Visual Studio-PCL-Projekt")](images/image3.png#lightbox)


Die PCL ist nun bereit für Visual Basic-Code hinzugefügt werden. PCL-Projekte können von anderen Projekten (Anwendungsprojekte, Bibliotheksprojekte und auch andere PCL-Projekte) verwiesen werden.

### <a name="editing-the-pcl-profile"></a>Bearbeiten das PCL-Profil

Dem PCL-Profil (die steuert, welche Plattformen die PCL ist kompatibel mit) angezeigt und geändert, indem Sie mit der rechten Maustaste auf das Projekt, und wählen können **Eigenschaften > Bibliothek > ändern...** . Im daraufhin angezeigten Dialogfeld ist in diesem Screenshot gezeigt:

 [![](images/image4-sml.png "PCL-Profil in den Projekteigenschaften bearbeiten")](images/image4.png#lightbox)

Wenn das Profil geändert wird, nachdem der Code bereits zu der PCL hinzugefügt wurde, ist es möglich, dass die Bibliothek nicht mehr kompiliert wird, wenn der Code verweist auf Funktionen, die nicht Teil des neu ausgewählten Profils sind.


## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde veranschaulicht, wie Visual Basic-Code in Xamarin-Anwendungen mit Visual Studio und Portable Class Libraries genutzt. Obwohl Xamarin Visual Basic nicht direkt unterstützt, ermöglicht das Kompilieren von Visual Basic in einer PCL Code mit Visual Basic, um in iOS und Android-apps enthalten sein.

Die folgenden Seiten beschrieben, wie Visual Basic.NET portable Klassenbibliotheken im einheitlichen Modus oder Xamarin.Forms-apps:

- [Erstellen von systemeigenen apps für Xamarin.iOS und Xamarin.Android, mit denen VB](native-apps.md)
- [Erstellen von Xamarin.Forms-apps mit VB](xamarin-forms.md)


## <a name="related-links"></a>Verwandte Links

- [TaskyPortableVB (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/TaskyPortableVB)
- [XamarinFormsVB (sample)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/XamarinFormsVB)
- [Plattformübergreifende Entwicklung mit .NET Framework (Microsoft)](https://msdn.microsoft.com/library/gg597391(v=vs.110).aspx)
