---
title: Binden eines Eclipse-Bibliotheksprojekts
description: In dieser exemplarischen Vorgehensweise wird erläutert, wie Xamarin.Android-Projektvorlagen verwenden eine Eclipse-Android-Bibliotheksprojekt gebunden wird.
ms.prod: xamarin
ms.assetid: CEE90F8A-164B-4155-813A-7537A665A7E7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: a5887748eda8af89b9215e3e121db592dfa73935
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "60957628"
---
# <a name="binding-an-eclipse-library-project"></a>Binden eines Eclipse-Bibliotheksprojekts

_In dieser exemplarischen Vorgehensweise wird erläutert, wie Xamarin.Android-Projektvorlagen verwenden eine Eclipse-Android-Bibliotheksprojekt gebunden wird._


## <a name="overview"></a>Übersicht

Obwohl. Zusätzlich zum AAR-Dateien Produktivitätsgründen zunehmend die Norm für die Verteilung von Android-Bibliothek, in einigen Fällen ist es erforderlich, erstellen eine Bindung für ein *Android-Bibliotheksprojekt*. Bibliothek für Android-Projekte sind spezielle Android-Projekte, die freigegebenen Code enthalten und Ressourcen, die von Android-Anwendungsprojekten verwiesen werden können. In der Regel binden Sie an eine Android-Bibliotheksprojekt. wenn die Bibliothek in der Eclipse-IDE erstellt wird.
Diese exemplarische Vorgehensweise enthält Beispiele dafür, wie ein Android-Bibliotheksprojekt zu erstellen. ZIP in der Verzeichnisstruktur des eines Ecplise-Projekts.

Android-Library-Projekten unterscheiden sich von reguläre Android-Projekte, insofern sie nicht in ein APK kompiliert und befasst sich nicht allein auf einem Gerät bereitgestellt. Stattdessen soll ein Android-Bibliotheksprojekt ein Android-Anwendungsprojekt verwiesen werden. Wenn ein Android-Anwendungsprojekt erstellt wird, wird die Android-Bibliotheksprojekt zuerst kompiliert. Der Android-Anwendungsprojekt wird dann in der kompilierten Android-Bibliotheksprojekt Drehmoment übernommen werden und enthalten den Code und die Ressourcen in der APK für die Verteilung. Aufgrund dieses Unterschieds ist das Erstellen einer Bindung für ein Android-Bibliotheksprojekt etwas anders als das Erstellen einer Bindung für eine Java. JAR- oder. Zusätzlich zum AAR-Datei.



## <a name="walkthrough"></a>Exemplarische Vorgehensweise

Um ein Android-Bibliotheksprojekt in einem Projekt Xamarin.Android Java Bindung zu verwenden ist es zunächst notwendig, um die Bibliothek für Android-Projekt in Eclipse zu erstellen. Der folgende Screenshot zeigt ein Beispiel für ein Projekt für Android-Bibliothek nach der Kompilierung: 

[![Beispiel-Library-Projekt in Eclipse](binding-a-library-project-images/build-lib-in-eclipse.png)](binding-a-library-project-images/build-lib-in-eclipse.png#lightbox)

Beachten Sie, dass der Quellcode, aus der Bibliothek für Android-Projekt in ein temporäres kompiliert wurde. JAR-Datei mit dem Namen **Android-mapviewballoons.jar**, und die Ressourcen auf kopiert wurden, die **"bin" / Res/aufschlüsseln** Ordner. 

Nachdem das Projekt für Android-Bibliothek in Eclipse kompiliert wurde, kann es dann mithilfe eines Xamarin.Android Binden von Java-Projekts gebunden werden. Erste ein. ZIP-Datei muss erstellt werden, enthält die **Bin** und **Res** Ordner des Projekts für Android-Bibliothek. Es ist wichtig, die Sie entfernen die dazwischen liegenden **aufschlüsseln** Unterverzeichnis, damit die Ressourcen in befinden **"bin" / Res**. Der folgende Screenshot zeigt den Inhalt eines solchen. ZIP-Datei: 

[![Inhalt der ZIP-Datei mit Android-Bibliothek](binding-a-library-project-images/contents-of-zip-file.png)](binding-a-library-project-images/contents-of-zip-file.png#lightbox)

Dies. ZIP-Datei wird dann zum Binden von Xamarin.Android-Java-Projekt hinzugefügt, wie im folgenden Screenshot gezeigt:

[![Binden von Java-Projekt hinzugefügt, ZIP](binding-a-library-project-images/zip-in-binding-project.png)](binding-a-library-project-images/zip-in-binding-project.png#lightbox)

Beachten Sie, dass die Buildaktion der. ZIP-Datei wurde automatisch auf gesetzt **LibraryProjectZip**.

Wenn Sie vorhanden sind. JAR-Dateien, die von der Bibliothek für Android-Projekt erforderlich sind, diese hinzugefügt werden sollen die **JAR-Dateien** -Ordner des Projekts Binden von Java-Bibliothek und die **Buildvorgang** festgelegt **ReferenceJar**. Im folgenden Screenshot sehen ein Beispiel hierfür: 

[![Legen Sie auf ReferenceJar Buildvorgang](binding-a-library-project-images/set-to-referencejar.png)](binding-a-library-project-images/set-to-referencejar.png#lightbox)

Nachdem diese Schritte abgeschlossen haben, kann das Binden von Xamarin.Android-Java-Projekt verwendet werden, wie im weiter oben in diesem Dokument beschrieben.

> [!NOTE]
> Kompilieren die Bibliothek für Android-Projekte in anderen IDEs ist zu diesem Zeitpunkt nicht unterstützt. Andere IDEs kann nicht erstellt werden, die derselben Verzeichnisstruktur oder in der **Bin** Ordner wie Eclipse. 


## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie wir den Prozess der Bindung ein Android-Bibliotheksprojekt. Erstellt die Bibliothek für Android-Projekt in Eclipse, und es erstellt eine Zip-Datei aus dem **Bin** und **Res** Ordner des Projekts für Android-Bibliothek. Als Nächstes verwendet haben wir diese ZIP-Datei zum Erstellen eines Projekts Xamarin.Android Java Bindung. 

