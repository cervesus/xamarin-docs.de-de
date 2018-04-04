---
title: Kann die Xamarin.Forms-Standardvorlage auf eine neuere NuGet-Paket werden aktualisiert?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 160FBE13-26EB-4B4F-9248-A5CBE58FDD7F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: fce595d7722dcd053f6fc9dcad84dc9a921e55b3
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="can-i-update-the-xamarinforms-default-template-to-a-newer-nuget-package"></a>Kann die Xamarin.Forms-Standardvorlage auf eine neuere NuGet-Paket werden aktualisiert?

Dieses Handbuch verwendet die Xamarin.Forms-PCL-Vorlage als Beispiel, aber die gleiche allgemeine Methode funktioniert auch für die freigegebene Xamarin.Forms-Projektvorlage. Dieses Handbuch geschrieben wird im Beispiel für das Aktualisieren von Xamarin.Forms 1.5.1.6471 zu 2.1.0.6529, jedoch die gleichen Schritte möglich, andere Versionen stattdessen als Standard festzulegen.

1.  Kopieren Sie die Originalvorlage `.zip` aus:

    > `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Xamarin\Xamarin\[Xamarin Version]\T\PT\Cross-Platform\Xamarin.Forms.PCL.zip`

2.  Entpacken Sie die `.zip` in ein temporäres Verzeichnis.

3.  Ändern Sie alle Vorkommen der alten Version des Pakets Formulare in die neue Version, die Sie verwenden möchten.
    *   `FormsTemplate\FormsTemplate.vstemplate`
    *   `FormsTemplate.Android\FormsTemplate.Android.vstemplate`
    *   `FormsTemplate.WinPhone\FormsTemplate.WinPhone.vstemplate`
    *   `FormsTemplate.iOS\FormsTemplate.iOS.vstemplate`

    Beispiel: `<package id="Xamarin.Forms" version="1.5.1.6471" />` -> `<package id="Xamarin.Forms" version="2.1.0.6529" />`

4.  Ändern Sie das Element "Name" der Hauptelemente [mit mehreren Projekten Vorlagendatei](http://msdn.microsoft.com/library/ms185308.aspx) (`Xamarin.Forms.PCL.vstemplate`) um die Eindeutigkeit herzustellen. Zum Beispiel:
    > <Name>Leere App (Xamarin.Forms Portable) - 2.1.0.6529</Name>

5.  Erneut zippen Sie gesamte Vorlagenordner. Stellen Sie sicher, dass die ursprüngliche Dateistruktur übereinstimmen. die `.zip` Datei. Die `Xamarin.Forms.PCL.vstemplate` Datei muss sich am Anfang der `.zip` Datei nicht im Ordner.

6.  Erstellen Sie ein Unterverzeichnis "Mobile-Apps" im Ordner Einzelbenutzer-Visual Studio-Vorlagen:
    > `%USERPROFILE%\Documents\Visual Studio 2013\Templates\ProjectTemplates\Visual C#\Mobile Apps`

7.  Kopieren Sie die neue ZIP-nach-oben-Vorlagenordner in das neue Verzeichnis der "Mobile-Apps".

8.  Laden Sie das NuGet-Paket, das die Version aus Schritt 3 entspricht. Beispielsweise [ http://nuget.org/api/v2/package/Xamarin.Forms/2.1.0.6529 ](http://nuget.org/api/v2/package/Xamarin.Forms/2.1.0.6529) (Siehe auch [ http://stackoverflow.com/questions/8597375/how-to-get-the-url-of-a-nupkg-file ](http://stackoverflow.com/questions/8597375/how-to-get-the-url-of-a-nupkg-file)), und kopieren Sie ihn in den entsprechenden Unterordner des Ordners Xamarin für Visual Studio-Erweiterungen:
    > `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Xamarin\Xamarin\[Xamarin Version]\Packages`
