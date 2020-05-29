---
title: Kann ich die Xamarin.Forms Standardvorlage auf ein neueres nuget-Paket aktualisieren?
ms.topic: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: bdead80671a1ae6539de6614441df7e86863a5a6
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137474"
---
# <a name="can-i-update-the-xamarinforms-default-template-to-a-newer-nuget-package"></a>Kann ich die Xamarin.Forms Standardvorlage auf ein neueres nuget-Paket aktualisieren?

In dieser Anleitung Xamarin.Forms wird die Vorlage für die .NET Standard-Bibliothek als Beispiel verwendet, aber dieselbe allgemeine Methode funktioniert auch für die Vorlage für frei Xamarin.Forms gegebene Projekte. Dieses Handbuch ist mit einem Beispiel für die Aktualisierung von Xamarin.Forms 1.5.1.6471 auf 2.1.0.6529 geschrieben, aber es ist möglich, stattdessen andere Versionen als Standard festzulegen.

1. Kopieren Sie die ursprüngliche Vorlage `.zip` aus:

    > `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Xamarin\Xamarin\[Xamarin Version]\T\PT\Cross-Platform\Xamarin.Forms.PCL.zip`

2. Entzippen `.zip` Sie an einem temporären Speicherort.

3. Ändern Sie alle Vorkommen der alten Version des Xamarin.Forms Pakets in die neue Version, die Sie verwenden möchten.
    * `FormsTemplate\FormsTemplate.vstemplate`
    * `FormsTemplate.Android\FormsTemplate.Android.vstemplate`
    * `FormsTemplate.iOS\FormsTemplate.iOS.vstemplate`

    Beispiel: `<package id="Xamarin.Forms" version="1.5.1.6471" />` -> `<package id="Xamarin.Forms" version="2.1.0.6529" />`

4. Ändern Sie das "Name"-Element der Haupt [Vorlagen Datei für mehrere Projekte](https://msdn.microsoft.com/library/ms185308.aspx) ( `Xamarin.Forms.PCL.vstemplate` ), um es eindeutig zu machen. Beispiel:

    > `<Name>Blank App (Xamarin.Forms Portable) - 2.1.0.6529</Name>`

5. Komprimieren Sie den gesamten Vorlagen Ordner neu. Stellen Sie sicher, dass Sie der ursprünglichen Dateistruktur der `.zip` Datei entsprechen. Die `Xamarin.Forms.PCL.vstemplate` Datei sollte sich am Anfang der Datei befinden `.zip` , nicht in einem Ordner.

6. Erstellen Sie im Ordner "Visual Studio-Vorlagen" pro Benutzer ein Unterverzeichnis "Mobile Apps":
    > `%USERPROFILE%\Documents\Visual Studio 2013\Templates\ProjectTemplates\Visual C#\Mobile Apps`

7. Kopieren Sie den neuen ZIP-Vorlagen Ordner in das neue Verzeichnis "Mobile Apps".

8. Laden Sie das nuget-Paket herunter, das mit der Version aus Schritt 3 übereinstimmt. Beispielsweise [ https://nuget.org/api/v2/package/ Xamarin.Forms /2.1.0.6529](https://nuget.org/api/v2/package/Xamarin.Forms/2.1.0.6529) (siehe auch [https://stackoverflow.com/questions/8597375/how-to-get-the-url-of-a-nupkg-file](https://stackoverflow.com/questions/8597375/how-to-get-the-url-of-a-nupkg-file) ), und kopieren Sie die Datei in den entsprechenden Unterordner des Ordners "Visual Studio-Erweiterungen" von xamarin:
    > `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Xamarin\Xamarin\[Xamarin Version]\Packages`
