---
title: Kann ich die Xamarin.Forms-Standardvorlage auf ein neueres NuGet-Paket aktualisieren?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 160FBE13-26EB-4B4F-9248-A5CBE58FDD7F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 4a628deb3e6f9282d49d71ac694506c3a0616ee9
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73005387"
---
# <a name="can-i-update-the-xamarinforms-default-template-to-a-newer-nuget-package"></a>Kann ich die Xamarin.Forms-Standardvorlage auf ein neueres NuGet-Paket aktualisieren?

In dieser Anleitung wird die xamarin. Forms-.NET Standard Bibliotheks Vorlage als Beispiel verwendet, aber dieselbe allgemeine Methode funktioniert auch für die freigegebene xamarin. Forms-Projektvorlage. Dieses Handbuch ist mit dem Beispiel für das Aktualisieren von xamarin. Forms 1.5.1.6471 auf 2.1.0.6529 geschrieben, aber es ist möglich, stattdessen andere Versionen als Standard festzulegen.

1. Kopieren Sie den ursprünglichen Vorlagen `.zip` aus:

    > `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Xamarin\Xamarin\[Xamarin Version]\T\PT\Cross-Platform\Xamarin.Forms.PCL.zip`

2. Entpacken Sie die `.zip` an einem temporären Speicherort.

3. Ändern Sie alle Vorkommen der alten Version des xamarin. Forms-Pakets in die neue Version, die Sie verwenden möchten.
    * `FormsTemplate\FormsTemplate.vstemplate`
    * `FormsTemplate.Android\FormsTemplate.Android.vstemplate`
    * `FormsTemplate.iOS\FormsTemplate.iOS.vstemplate`

    Beispiel: `<package id="Xamarin.Forms" version="1.5.1.6471" />` -> `<package id="Xamarin.Forms" version="2.1.0.6529" />`

4. Ändern Sie das "Name"-Element der Haupt [Vorlagen Datei für mehrere Projekte](https://msdn.microsoft.com/library/ms185308.aspx) (`Xamarin.Forms.PCL.vstemplate`), um Sie eindeutig zu machen. Beispiel:

    > `<Name>Blank App (Xamarin.Forms Portable) - 2.1.0.6529</Name>`

5. Komprimieren Sie den gesamten Vorlagen Ordner neu. Stellen Sie sicher, dass die ursprüngliche Dateistruktur der `.zip` Datei entspricht. Die `Xamarin.Forms.PCL.vstemplate` Datei sollte sich im oberen Bereich der Datei `.zip` befinden, nicht in einem Ordner.

6. Erstellen Sie im Ordner "Visual Studio-Vorlagen" pro Benutzer ein Unterverzeichnis "Mobile Apps":
    > `%USERPROFILE%\Documents\Visual Studio 2013\Templates\ProjectTemplates\Visual C#\Mobile Apps`

7. Kopieren Sie den neuen ZIP-Vorlagen Ordner in das neue Verzeichnis "Mobile Apps".

8. Laden Sie das nuget-Paket herunter, das mit der Version aus Schritt 3 übereinstimmt. Beispielsweise [https://nuget.org/api/v2/package/Xamarin.Forms/2.1.0.6529](https://nuget.org/api/v2/package/Xamarin.Forms/2.1.0.6529) (siehe auch [https://stackoverflow.com/questions/8597375/how-to-get-the-url-of-a-nupkg-file](https://stackoverflow.com/questions/8597375/how-to-get-the-url-of-a-nupkg-file)), und kopieren Sie Sie in den entsprechenden Unterordner des Ordners "Visual Studio-Erweiterungen" von xamarin:
    > `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Xamarin\Xamarin\[Xamarin Version]\Packages`
