---
title: Binden eines Eclipse-Bibliotheksprojekts
description: In dieser exemplarischen Vorgehensweise wird erläutert, wie Sie mit xamarin. Android-Projektvorlagen ein Eclipse-Android-Bibliotheksprojekt binden.
ms.prod: xamarin
ms.assetid: CEE90F8A-164B-4155-813A-7537A665A7E7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 78b70ce70292e589aee4a1dbe56f3765552ece7a
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757721"
---
# <a name="binding-an-eclipse-library-project"></a>Binden eines Eclipse-Bibliotheksprojekts

_In dieser exemplarischen Vorgehensweise wird erläutert, wie Sie mit xamarin. Android-Projektvorlagen ein Eclipse-Android-Bibliotheksprojekt binden._

## <a name="overview"></a>Übersicht

Auch. AAR-Dateien werden zunehmend zur Norm für die Verteilung von Android-Bibliotheken. in einigen Fällen ist es erforderlich, eine Bindung für ein *Android-Bibliotheksprojekt*zu erstellen. Android-Bibliotheks Projekte sind spezielle Android-Projekte, die Share baren Code und Ressourcen enthalten, auf die von Android-Anwendungsprojekten verwiesen werden kann. In der Regel Binden Sie an ein Android-Bibliotheksprojekt, wenn die Bibliothek in der Eclipse-IDE erstellt wird.
Diese exemplarische Vorgehensweise enthält Beispiele, wie Sie ein Android-Bibliotheksprojekt erstellen. ZIP von der Verzeichnisstruktur eines Eclipse-Projekts.

Android-Bibliotheks Projekte unterscheiden sich von regulären Android-Projekten darin, dass Sie nicht in ein APK kompiliert werden und nicht eigenständig für ein Gerät bereitgestellt werden können. Stattdessen soll von einem Android-Anwendungsprojekt auf ein Android-Bibliotheksprojekt verwiesen werden. Wenn ein Android-Anwendungsprojekt erstellt wird, wird das Android-Bibliotheksprojekt zuerst kompiliert. Das Android-Anwendungsprojekt wird dann in das kompilierte Android-Bibliotheksprojekt aufgenommen und enthält den Code und die Ressourcen in das APK für die Verteilung. Aufgrund dieses Unterschieds unterscheidet sich das Erstellen einer Bindung für ein Android-Bibliotheksprojekt etwas von der Erstellung einer Bindung für ein Java-Projekt. JAR oder. AAR-Datei.

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

Wenn Sie ein Android-Bibliotheksprojekt in einem xamarin. Android-Java-Bindungs Projekt verwenden möchten, muss zunächst das Android-Bibliotheksprojekt in Eclipse erstellt werden. Der folgende Screenshot zeigt ein Beispiel für ein Android-Bibliotheksprojekt nach der Kompilierung: 

[![Beispiel Bibliotheksprojekt in Eclipse](binding-a-library-project-images/build-lib-in-eclipse.png)](binding-a-library-project-images/build-lib-in-eclipse.png#lightbox)

Beachten Sie, dass der Quellcode aus dem Android-Bibliotheksprojekt in einen temporären kompiliert wurde. JAR-Datei mit dem Namen **Android-mapviewballoons. jar**, und die Ressourcen wurden in den Ordner **bin/res/Crunch** kopiert. 

Nachdem das Android-Bibliotheksprojekt in Eclipse kompiliert wurde, kann es mit einem xamarin. Android-Java-Bindungs Projekt gebunden werden. Zuerst ein. Die ZIP-Datei muss erstellt werden, die die Ordner " **bin** " und " **Res** " des Android-Bibliotheks Projekts enthält. Es ist wichtig, dass Sie das dazwischen liegende Unterverzeichnis " **Crunch** " entfernen, damit sich die Ressourcen in " **bin/res**" befinden. Der folgende Screenshot zeigt den Inhalt eines solchen. ZIP-Datei: 

[![Inhalt der Android-Bibliothek Project. zip](binding-a-library-project-images/contents-of-zip-file.png)](binding-a-library-project-images/contents-of-zip-file.png#lightbox)

Dafür. Die ZIP-Datei wird dann dem Java-Bindungs Projekt xamarin. Android hinzugefügt, wie im folgenden Screenshot zu sehen:

[![ZIP-Hinzufügung zum Java-Bindungs Projekt](binding-a-library-project-images/zip-in-binding-project.png)](binding-a-library-project-images/zip-in-binding-project.png#lightbox)

Beachten Sie, dass die Buildaktion des. Die ZIP-Datei wurde automatisch auf **libraryprojectzip**festgelegt.

Wenn vorhanden. JAR-Dateien, die für das Android-Bibliotheksprojekt erforderlich sind, müssen dem Ordner " **Jars** " des Projekts für die Java-Bindungs Bibliothek und der **Buildaktion** **referencejar**hinzugefügt werden. Ein Beispiel hierfür finden Sie im folgenden Screenshot: 

[![Buildaktion auf referencejar festgelegt](binding-a-library-project-images/set-to-referencejar.png)](binding-a-library-project-images/set-to-referencejar.png#lightbox)

Nachdem diese Schritte ausgeführt wurden, kann das xamarin. Android-Java-Bindungs Projekt verwendet werden, wie weiter oben in diesem Dokument beschrieben.

> [!NOTE]
> Das Kompilieren von Android-Bibliotheks Projekten in anderen IDEs wird zurzeit nicht unterstützt. Andere IDES erstellen möglicherweise nicht die gleiche Verzeichnisstruktur oder die gleichen Dateien im Ordner " **bin** " wie Eclipse. 

## <a name="summary"></a>Zusammenfassung

In diesem Artikel haben wir den Prozess der Bindung eines Android-Bibliotheks Projekts erläutert. Wir haben das Android-Bibliotheksprojekt in Eclipse erstellt und dann eine ZIP-Datei aus dem Ordner " **bin** " und " **Res** " des Android-Bibliotheks Projekts erstellt. Als nächstes verwenden wir diese ZIP-Datei, um ein xamarin. Android-Java-Bindungs Projekt zu erstellen. 
