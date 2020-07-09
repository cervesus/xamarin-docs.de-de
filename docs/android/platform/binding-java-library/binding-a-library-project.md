---
title: Binden eines Eclipse-Bibliotheksprojekts
description: In dieser exemplarischen Vorgehensweise wird erläutert, wie Sie mit Xamarin.Android-Projektvorlagen ein Eclipse-Android-Bibliotheksprojekt binden.
ms.prod: xamarin
ms.assetid: CEE90F8A-164B-4155-813A-7537A665A7E7
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: 54de1cde65e9c1ab9cb758c057a87c6b15c4da54
ms.sourcegitcommit: a3f13a216fab4fc20a9adf343895b9d6a54634a5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85853029"
---
# <a name="binding-an-eclipse-library-project"></a>Binden eines Eclipse-Bibliotheksprojekts

> [!IMPORTANT]
> Wir untersuchen derzeit die Nutzung benutzerdefinierter Bindungen auf der Xamarin-Plattform. Nehmen Sie an [**dieser Umfrage**](https://www.surveymonkey.com/r/KKBHNLT) teil, um zukünftige Entwicklungsarbeiten zu unterstützen.

_In dieser exemplarischen Vorgehensweise wird erläutert, wie Sie mit Xamarin.Android-Projektvorlagen ein Eclipse-Android-Bibliotheksprojekt binden._

## <a name="overview"></a>Übersicht

Obwohl AAR-Dateien zunehmend zur Normalfall für die Verteilung von Android-Bibliotheken werden, ist es in einigen Fällen erforderlich, eine Bindung für ein *Android-Bibliotheksprojekt* zu erstellen. Android-Bibliotheksprojekte sind spezielle Android-Projekte, die freigabefähigen Code und Ressourcen enthalten, auf die von Android-Anwendungsprojekten verwiesen werden kann. In der Regel binden Sie ein Android-Bibliotheksprojekt, wenn die Bibliothek in der Eclipse-IDE erstellt wird.
Diese exemplarische Vorgehensweise enthält Beispiele zum Erstellen einer ZIP-Datei für das Android-Bibliotheksprojekt aus der Verzeichnisstruktur eines Eclipse-Projekts.

Android-Bibliotheksprojekte unterscheiden sich insofern von regulären Android-Projekten, als dass sie nicht in ein APK kompiliert werden und nicht eigenständig auf einem Gerät bereitgestellt werden können. Stattdessen soll ein Android-Anwendungsprojekt auf ein Android-Bibliotheksprojekt verweisen. Wenn ein Android-Anwendungsprojekt erstellt wird, wird zuerst das Android-Bibliotheksprojekt kompiliert. Das Android-Anwendungsprojekt wird dann in das kompilierte Android-Bibliotheksprojekt aufgenommen und bezieht den Code und die Ressourcen in das APK für die Verteilung ein. Aufgrund dieses Unterschieds weicht das Erstellen einer Bindung für ein Android-Bibliotheksprojekt geringfügig vom Erstellen einer Bindung für eine JAR- oder AAR-Java-Datei ab.

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

Wenn Sie ein Android-Bibliotheksprojekt in einem Xamarin.Android-Java-Bindungsprojekt verwenden möchten, muss zunächst das Android-Bibliotheksprojekt in Eclipse erstellt werden. Der folgende Screenshot zeigt ein Beispiel für ein Android-Bibliotheksprojekt nach der Kompilierung: 

[![Beispiel für ein Bibliotheksprojekt in Eclipse](binding-a-library-project-images/build-lib-in-eclipse.png)](binding-a-library-project-images/build-lib-in-eclipse.png#lightbox)

Beachten Sie, dass der Quellcode aus dem Android-Bibliotheksprojekt in eine temporäre Die JAR-Datei namens **android-mapviewballoons.jar** kompiliert wurde und die Ressourcen in den Ordner **bin/res/crunch** kopiert wurden. 

Nachdem das Android-Bibliotheksprojekt in Eclipse kompiliert wurde, kann es mit einem Xamarin.Android-Java-Bindungsprojekt gebunden werden. Zuerst muss eine ZIP-Datei erstellt werden, die die Ordner **bin** und **res** des Android-Bibliotheksprojekts enthält. Sie müssen unbedingt das Unterverzeichnis **crunch** dazwischen entfernen, damit sich die Ressourcen in **bin/res** befinden. Der folgende Screenshot zeigt den Inhalt eines solchen ZIP-Datei: 

[![Inhalt der ZIP-Datei für das Android-Bibliotheksprojekt](binding-a-library-project-images/contents-of-zip-file.png)](binding-a-library-project-images/contents-of-zip-file.png#lightbox)

Diese ZIP-Datei wird dann wie im folgenden Screenshot gezeigt zum Xamarin.Android-Java-Bindungsprojekt hinzugefügt:

[![Hinzufügen der ZIP-Datei zum Java-Bindungsprojekt](binding-a-library-project-images/zip-in-binding-project.png)](binding-a-library-project-images/zip-in-binding-project.png#lightbox)

Beachten Sie, dass der Buildvorgang der ZIP-Datei automatisch auf **LibraryProjectZip** festgelegt wurde.

Wenn JAR-Dateien für das Android-Bibliotheksprojekt erforderlich sind, müssen sie zum Ordner **Jars** des Java-Bindungsbibliothekprojekts hinzugefügt werden, und der **Buildvorgang** muss auf **ReferenceJar** festgelegt werden. Ein entsprechendes Beispiel finden Sie im folgenden Screenshot: 

[![Festlegen des Buildvorgangs auf ReferenceJar](binding-a-library-project-images/set-to-referencejar.png)](binding-a-library-project-images/set-to-referencejar.png#lightbox)

Nachdem diese Schritte ausgeführt wurden, kann das Xamarin.Android-Java-Bindungsprojekt wie weiter oben in diesem Dokument beschrieben verwendet werden.

> [!NOTE]
> Das Kompilieren von Android-Bibliotheksprojekten in anderen IDEs wird zurzeit nicht unterstützt. Andere IDEs erstellen u. U. nicht die gleiche Verzeichnisstruktur oder die gleichen Dateien im Ordner **bin** wie Eclipse. 

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde die Bindung eines Android-Bibliotheksprojekts in einer exemplarischen Vorgehensweise erläutert. Das Android-Bibliotheksprojekt wurde in Eclipse erstellt. Anschließend wurde eine ZIP-Datei aus den Ordnern **bin** und **res** des Android-Bibliotheksprojekts erstellt. Als nächstes wurde diese ZIP-Datei zum Erstellen eines Xamarin.Android-Java-Bindungsprojekts verwendet. 
