---
title: Binden ein Projekt der Eclipse-Bibliothek
description: In dieser exemplarischen Vorgehensweise wird erläutert, wie Xamarin.Android-Projektvorlagen zum Binden eines Eclipse Android Library-Projekts verwenden wird.
ms.prod: xamarin
ms.assetid: CEE90F8A-164B-4155-813A-7537A665A7E7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 1c3c33f1b958aff5818b26e4408e60f449b46f41
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
ms.locfileid: "30764507"
---
# <a name="binding-an-eclipse-library-project"></a>Binden ein Projekt der Eclipse-Bibliothek

_In dieser exemplarischen Vorgehensweise wird erläutert, wie Xamarin.Android-Projektvorlagen zum Binden eines Eclipse Android Library-Projekts verwenden wird._


## <a name="overview"></a>Übersicht

Obwohl. AAR-Dateien werden in zunehmendem Norm für die Bibliothek für Android-Verteilung, in einigen Fällen ist es erforderlich, erstellen Sie eine Bindung für eine *Bibliothek für Android-Projekt*. Bibliothek für Android-Projekte sind spezielle Android-Projekte, die freigegeben Code enthalten und Ressourcen, die mit Android-Anwendungsprojekten verwiesen werden können. In der Regel binden Sie eine Bibliothek für Android-Projekt, wenn die Bibliothek in der Eclipse-IDE erstellt wird.
Diese exemplarische Vorgehensweise enthält Beispiele zum Erstellen einer Bibliothek für Android-Projekt. ZIP aus Verzeichnisstruktur des Eclipse-Projekt.

Bibliothek für Android-Projekten unterscheiden sich von regulären Android-Projekte, insofern, dass sie nicht in ein APK kompiliert werden und nicht, Ihre eigenen für ein Gerät bereitgestellt. Stattdessen soll eine Bibliothek für Android-Projekt von einer Android-Anwendungsprojekt verwiesen werden. Wenn ein Projekt für Android-Anwendung erstellt wird, wird die Bibliothek für Android-Projekt zuerst kompiliert. Das Android-Anwendungsprojekt wird dann in der kompilierten Bibliothek für die Android-Projekt Drehmoment übernommen werden und enthalten den Code und die Ressourcen in der APK für die Verteilung. Wegen dieses Unterschieds ist das Erstellen einer Bindung für eine Bibliothek für Android-Projekt unterscheidet sich etwas davon ab, erstellen eine Bindung für eine Java. JAR oder. AAR-Datei.



## <a name="walkthrough"></a>Exemplarische Vorgehensweise

Um eine Bibliothek für Android-Projekt in einem Projekt Xamarin.Android Java Bindung zu verwenden ist es ersten erforderlich, um die Bibliothek für Android-Projekt in Eclipse zu erstellen. Der folgende Screenshot zeigt ein Beispiel für eine Bibliothek für Android-Projekt nach der Kompilierung: 

[![Beispiel-Bibliotheksprojekt in Eclipse](binding-a-library-project-images/build-lib-in-eclipse.png)](binding-a-library-project-images/build-lib-in-eclipse.png#lightbox)

Beachten Sie, dass es sich bei der Quellcode aus der Bibliothek für Android-Projekt an einen temporären kompiliert wurde. JAR-Datei mit dem Namen **Android mapviewballoons.jar**, und die Ressourcen kopiert wurden, um die **"bin" / Res/Druck zu nehmen** Ordner. 

Sobald die Bibliothek für Android-Projekt in Eclipse kompiliert wurde, kann es dann mithilfe eines Projekts Xamarin.Android Java Bindung gebunden werden. Erste ein. ZIP-Datei muss erstellt werden, enthält die **"bin"** und **Res** Ordner des Projekts für Android-Bibliothek. Es ist wichtig, dass Sie die dazwischen liegende entfernen **Druck zu nehmen** Unterverzeichnis, damit in dem sich die Ressourcen befinden **"bin" / Res**. Der folgende Screenshot zeigt den Inhalt eines solchen. ZIP-Datei: 

[![Inhalt der ZIP der Bibliothek für Android-Projekt](binding-a-library-project-images/contents-of-zip-file.png)](binding-a-library-project-images/contents-of-zip-file.png#lightbox)

Dies. ZIP-Datei wird dann Xamarin.Android Binden von Java-Projekt hinzugefügt, wie im folgenden Screenshot gezeigt:

[![Binden von Java-Projekt hinzugefügt, ZIP](binding-a-library-project-images/zip-in-binding-project.png)](binding-a-library-project-images/zip-in-binding-project.png#lightbox)

Beachten Sie, dass der Buildvorgang für die. ZIP-Datei automatisch vorsieht **LibraryProjectZip**.

Wenn Sie vorhanden sind. JAR-Dateien, die die Bibliothek für Android-Projekts erforderlich sind, sie hinzugefügt werden sollen die **JAR-Dateien** Ordner, der das Binden von Java-Steuerelementbibliothek-Projekt und die **Buildvorgang** festgelegt **ReferenceJar**. Ein Beispiel hierfür kann im folgenden Screenshot angezeigt werden: 

[![Buildvorgang auf ReferenceJar festgelegt](binding-a-library-project-images/set-to-referencejar.png)](binding-a-library-project-images/set-to-referencejar.png#lightbox)

Nachdem Sie diese Schritte abgeschlossen haben, kann das Binden von Xamarin.Android Java-Projekt verwendet werden, wie im weiter oben in diesem Dokument beschrieben.

> [!NOTE]
> Kompilieren die Bibliothek für Android-Projekte in anderen IDEs wird zu diesem Zeitpunkt nicht unterstützt. Andere IDEs zu kann nicht erstellt werden, die gleiche Verzeichnisstruktur oder in der **"bin"** Ordner wie Eclipse. 


## <a name="summary"></a>Zusammenfassung

In diesem Artikel durchlaufen, wird durch den Prozess der Bindung einer Bibliothek für Android-Projekt. Wir haben das Bibliothek für Android-Projekt in Eclipse erstellt, dann es erstellt eine Zip-Datei aus der **"bin"** und **Res** Ordner des Projekts für Android-Bibliothek. Als Nächstes verwendet haben wir diese ZIP-Datei zum Erstellen eines Projekts Xamarin.Android Java Bindung. 

