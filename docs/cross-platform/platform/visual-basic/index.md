---
title: Visual Basic und .NET Standard
description: In dieser Anleitung wird erläutert, wie Visual Basic zum Schreiben von .NET Standard Projekten verwendet werden kann, die in Projektmappen für xamarin. IOS und xamarin. Android verwendet werden können.
ms.prod: xamarin
ms.assetid: f264c632-8feb-4015-a5e5-cb9c681c787d
author: conceptdev
ms.author: crdun
ms.date: 04/24/2019
ms.openlocfilehash: 1e58dd4d7310c7f903433ce0b84123c39fbb5c8e
ms.sourcegitcommit: f8583585c501607fdfa061b95e9a9f385ed1d591
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2019
ms.locfileid: "72959111"
---
# <a name="visual-basic-and-net-standard"></a>Visual Basic und .NET Standard

Xamarin Android-und IOS-Projekte unterstützen Visual Basic nicht. Entwickler können jedoch [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md) Bibliotheken verwenden, um vorhandenen Visual Basic Code zu Android und IOS zu migrieren oder einen signifikanten Teil der Anwendungslogik in Visual Basic zu schreiben. Xamarin. Forms-Anwendungen können vollständig in Visual Basic erstellt werden (ohne benutzerdefinierte Renderer, Abhängigkeits Dienste und XAML-Code Behind).

## <a name="requirements"></a>Anforderungen

Zum Erstellen und Kompilieren von Visual Basic .NET Standard Bibliotheken müssen Sie Visual Studio unter Windows (Visual Studio 2017 oder höher) verwenden.

> [!NOTE]
> Visual Basic Bibliotheken können nur mithilfe von Visual Studio erstellt und kompiliert werden. Xamarin. Android und xamarin. IOS unterstützen die Visual Basic Sprache nicht.
>
> Wenn Sie nur in Visual Studio arbeiten, können Sie auf das Visual Basic Projekt aus xamarin. Android-und xamarin. IOS-Projekten verweisen.
>
> Wenn Ihre Android-und IOS-Projekte ebenfalls in geladen werden müssen Visual Studio für Mac sollten Sie auf die Ausgabeassembly aus der Visual Basic-Assembly verweisen.

## <a name="creating-a-visual-basicnet-net-standard-library"></a>Erstellen einer Visual Basic.net-.NET Standard Bibliothek

In diesem Abschnitt wird erläutert, wie Sie mithilfe von Visual Studio 2019 eine Visual Basic .NET Standard Bibliothek erstellen.
Auf die Bibliothek kann dann in anderen Projekten, einschließlich xamarin. Android, xamarin. IOS und xamarin. Forms-apps, verwiesen werden.

Wenn Sie in Visual Studio eine Visual Basic .NET Standard Bibliothek hinzufügen, müssen Sie darauf achten, dass Sie den richtigen Projekttyp auswählen:

1. Wählen Sie in Visual Studio 2019 die Option **Neues Projekt erstellen**aus.

2. Geben Sie **Visual Basic Bibliothek** ein, um die Projektoptionen zu filtern, und wählen Sie die Option **Klassenbibliothek (.NET Standard)** mit dem Visual Basic Symbol aus:

    [![Filter für Visual Basic Bibliothek](xamarin-forms-images/06-sml.png)](xamarin-forms-images/06.png#lightbox)

3. Geben Sie auf dem nächsten Bildschirm einen Namen für das Projekt ein, und klicken Sie auf **Erstellen**.

4. Das Visual Basic Projekt wird wie folgt in der **Projektmappen-Explorer** angezeigt:

    [![leeres Visual Basic Projekt](images/new-library-sml.png)](images/new-library.png#lightbox)

Das Projekt ist nun bereit, Visual Basic Code hinzugefügt werden kann. Auf .NET Standard Projekte kann von anderen Projekten (Anwendungsprojekte oder Bibliotheks Projekte) verwiesen werden.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde gezeigt, wie Visual Basic Code in xamarin-Anwendungen mit Visual Studio verwendet werden kann. Obwohl xamarin Visual Basic nicht direkt unterstützt, ermöglicht die Kompilierung Visual Basic in eine .NET Standard Bibliothek das Einschließen von mit Visual Basic geschriebenen Code in Android-und IOS-apps.

Die folgenden Seiten beschreiben die Verwendung von Visual Basic.net-.NET Standard-Bibliotheken in nativen oder xamarin. Forms-apps:

- [Entwickeln nativer xamarin. IOS-und xamarin. Android-Apps, die VB verwenden](native-apps.md)
- [Entwickeln von xamarin. Forms-apps mit VB](xamarin-forms.md)

## <a name="related-links"></a>Verwandte Links

- [Taskyvb (Beispiel)](https://docs.microsoft.com/samples/xamarin/mobile-samples/visualbasic-taskyvb/)
- [Xamarinformsvb (Beispiel)](https://docs.microsoft.com/samples/xamarin/mobile-samples/visualbasic-xamarinformsvb/)
- [.NET Standard und xamarin](~/cross-platform/app-fundamentals/net-standard.md)
- [.NET-Standard](/dotnet/standard/net-standard/)
