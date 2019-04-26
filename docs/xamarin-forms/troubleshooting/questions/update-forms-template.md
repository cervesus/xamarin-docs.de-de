---
title: Kann ich die Xamarin.Forms-Standardvorlage auf ein neueres NuGet-Paket aktualisieren?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 160FBE13-26EB-4B4F-9248-A5CBE58FDD7F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: e439d39dd8591cad14485e64aabab2d6016a8e27
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61345908"
---
# <a name="can-i-update-the-xamarinforms-default-template-to-a-newer-nuget-package"></a>Kann ich die Xamarin.Forms-Standardvorlage auf ein neueres NuGet-Paket aktualisieren?

Dieser Anleitung wird die Vorlage für Xamarin.Forms .NET Standard-Bibliothek als Beispiel verwendet, aber die gleiche allgemeine Methode funktioniert auch für die Vorlage für freigegebene Xamarin.Forms-Projekt. Dieses Handbuch ist mit dem Beispiel der Aktualisierung von Xamarin.Forms 1.5.1.6471 zu 2.1.0.6529 geschrieben, aber die gleichen Schritte möglich, andere Versionen stattdessen als Standard festgelegt werden.

1.  Kopieren Sie die ursprüngliche Vorlage `.zip` aus:

    > `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Xamarin\Xamarin\[Xamarin Version]\T\PT\Cross-Platform\Xamarin.Forms.PCL.zip`

2.  Entzippen Sie die `.zip` an einen temporären Speicherort.

3.  Ändern Sie alle Vorkommen der die alte Version des Pakets Formulare, auf die neue Version, die Sie verwenden möchten.
    *   `FormsTemplate\FormsTemplate.vstemplate`
    *   `FormsTemplate.Android\FormsTemplate.Android.vstemplate`
    *   `FormsTemplate.iOS\FormsTemplate.iOS.vstemplate`

    Beispiel: `<package id="Xamarin.Forms" version="1.5.1.6471" />` -> `<package id="Xamarin.Forms" version="2.1.0.6529" />`

4.  Ändern Sie das Element "Name" der Haupt- [mit mehreren Projekten Vorlagendatei](https://msdn.microsoft.com/library/ms185308.aspx) (`Xamarin.Forms.PCL.vstemplate`) zum Sicherstellen der Eindeutigkeit. Zum Beispiel:
    > <Name>Leere App (Xamarin.Forms Portable) - 2.1.0.6529</Name>

5.  Zip-Ordner für die gesamte Vorlagen erneut. Achten Sie darauf, die der ursprünglichen Dateistruktur entsprechen den `.zip` Datei. Die `Xamarin.Forms.PCL.vstemplate` Datei muss sich am oberen Rand der `.zip` Datei nicht in Ordnern abgelegt.

6.  Erstellen Sie ein Unterverzeichnis "Mobile Apps" im Ordner pro Benutzer Visual Studio-Vorlagen:
    > `%USERPROFILE%\Documents\Visual Studio 2013\Templates\ProjectTemplates\Visual C#\Mobile Apps`

7.  Kopieren Sie den neuen Vorlagenordner für die komprimierte in das neue Verzeichnis der "Mobile Apps" an.

8.  Laden Sie das NuGet-Paket, das die Version aus Schritt 3 entspricht. Z. B. [ http://nuget.org/api/v2/package/Xamarin.Forms/2.1.0.6529 ](http://nuget.org/api/v2/package/Xamarin.Forms/2.1.0.6529) (Siehe auch [ https://stackoverflow.com/questions/8597375/how-to-get-the-url-of-a-nupkg-file ](https://stackoverflow.com/questions/8597375/how-to-get-the-url-of-a-nupkg-file)), und kopieren Sie ihn in den entsprechenden Unterordner des Ordners Xamarin Visual Studio-Erweiterungen:
    > `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Xamarin\Xamarin\[Xamarin Version]\Packages`
