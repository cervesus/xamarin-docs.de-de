---
title: Verwenden von freigegebenen Projekten zum Freigeben von Code
description: Mit freigegebenen Projekten können Sie allgemeinen Code schreiben, auf den von einer Reihe verschiedener Anwendungsprojekte verwiesen wird. Der Code wird als Teil jedes verweisenden Projekts kompiliert und kann Compilerdirektiven beinhalten, um plattformspezifische Funktionen in die freigegebene Codebasis zu integrieren.
ms.prod: xamarin
ms.assetid: 191c71fb-44a4-4e6c-af4b-7b1107dce6af
author: conceptdev
ms.author: crdun
ms.date: 07/18/2018
ms.openlocfilehash: ed58b0810d3c4fd3a3dd99cddd16227f9ac30273
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "68739057"
---
# <a name="shared-projects-code-sharing"></a>Freigegebene Projekte Code Freigabe

_Mit freigegebenen Projekten können Sie allgemeinen Code schreiben, auf den von einer Reihe verschiedener Anwendungsprojekte verwiesen wird. Der Code wird als Teil jedes verweisenden Projekts kompiliert und kann Compilerdirektiven einschließen, um die plattformspezifische Funktionalität in die freigegebene Codebasis zu integrieren._

Freigegebene Projekte (auch als freigegebene Ressourcen Projekte bezeichnet) ermöglichen Ihnen das Schreiben von Code, der von mehreren zielprojekten, einschließlich xamarin-Anwendungen, gemeinsam genutzt wird.

Sie unterstützen Compilerdirektiven, damit Sie plattformspezifischen Code bedingt einschließen können, der in eine Teilmenge der Projekte kompiliert werden soll, die auf das freigegebene Projekt verweisen. Es gibt auch IDE-Unterstützung, um die Compilerdirektiven zu verwalten und zu visualisieren, wie der Code in den einzelnen Anwendungen aussehen soll.

Wenn Sie in der Vergangenheit Datei Verknüpfungen für die gemeinsame Nutzung von Code zwischen Projekten verwendet haben, funktionieren freigegebene Projekte auf ähnliche Weise, aber mit einer deutlich verbesserten IDE-Unterstützung.

## <a name="what-is-a-shared-project"></a>Was ist ein frei gegebenes Projekt?

Im Gegensatz zu den meisten anderen Projekttypen verfügt ein frei gegebenes Projekt nicht über eine Ausgabe (in dll-Form), sondern der Code wird in jedes Projekt kompiliert, das darauf verweist. Dies wird im folgenden Diagramm veranschaulicht: konzeptionell wird der gesamte Inhalt des freigegebenen Projekts in jedes verweisende Projekt kopiert und so kompiliert, als wäre es ein Teil davon.

![](shared-projects-images/sharedassetproject.png "Architektur des freigegebenen Projekts")

Der Code in einem freigegebenen Projekt kann Compilerdirektiven enthalten, die Code Abschnitte abhängig vom Anwendungsprojekt, das den Code verwendet, aktivieren oder deaktivieren. Dies wird von den farbigen Platt Form Feldern im Diagramm vorgeschlagen.

Ein frei gegebenes Projekt wird nicht selbst kompiliert, sondern ist ausschließlich als Gruppierung von Quell Code Dateien vorhanden, die in andere Projekte eingeschlossen werden können. Wenn von einem anderen Projekt auf Sie verwiesen wird, wird der Code effektiv als *Teil* dieses Projekts kompiliert. Freigegebene Projekte können nicht auf einen anderen Projekttyp (einschließlich anderer frei gegebener Projekte) verweisen.

Beachten Sie, dass Android-Anwendungsprojekte nicht auf andere Android-Anwendungsprojekte verweisen können. beispielsweise kann ein Android-Komponenten Testprojekt nicht auf ein Android-Anwendungsprojekt verweisen. Weitere Informationen zu dieser Einschränkung finden Sie in dieser [Forumsdiskussion](http://forums.xamarin.com/discussion/comment/98092/).

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

## <a name="visual-studio-for-mac-walkthrough"></a>Exemplarische Vorgehensweise für Visual Studio für Mac

In diesem Abschnitt werden die Schritte zum Erstellen und Verwenden eines freigegebenen Projekts mit Visual Studio für Mac erläutert. Ein umfassendes Beispiel finden Sie im Abschnitt zu frei [gegebenen Projekten](#Shared_Project_Example) .

## <a name="creating-a-shared-project"></a>Erstellen eines freigegebenen Projekts

Um ein neues frei gegebenes Projekt zu erstellen, navigieren Sie zu **Datei > neue Projekt Mappe...** (oder klicken Sie mit der rechten Maustaste auf eine vorhandene Projekt Mappe, und wählen Sie **Hinzufügen > Neues Projekt hinzufügen**aus):

[![Neues] frei gegebenes Projekt (shared-projects-images/xs-newsolution-sml.png "Neue Lösung")](shared-projects-images/xs-newsolution.png#lightbox)

Wählen Sie auf dem nächsten Bildschirm den Projektnamen aus, und klicken Sie auf **Erstellen**.

Ein neues frei gegebenes Projekt wird unten angezeigt. Beachten Sie, dass es keine Verweise oder Komponenten Knoten gibt. Diese werden für freigegebene Projekte nicht unterstützt.

![Leeres] frei gegebenes Projekt (shared-projects-images/xs-empty.png "Leeres") frei gegebenes Projekt

Damit ein frei gegebenes Projekt nützlich ist, muss es von mindestens einem buildfähigen Projekt (z. b. einer IOS-oder Android-Anwendung oder-Bibliothek oder einem PCL-Projekt) referenziert werden. Ein frei gegebenes Projekt wird nicht kompiliert, wenn es nichts referenziert. Daher werden Syntax Fehler (oder andere) erst dann hervorgehoben, wenn von etwas anderem darauf verwiesen wird.

Das Hinzufügen eines Verweises auf ein frei gegebenes Projekt erfolgt auf die gleiche Weise wie das verweisen auf ein reguläres Bibliotheksprojekt. Dieser Screenshot zeigt ein xamarin. IOS-Projekt, das auf ein frei gegebenes Projekt verweist.

![](shared-projects-images/xs-reference.png "Projekt Verweis auf frei gegebenes Projekt")

Wenn von einer anderen Bibliothek oder Anwendung auf das freigegebene Projekt verwiesen wird, können Sie die Projekt Mappe erstellen und alle Fehler im Code anzeigen. Wenn von _zwei oder mehr_ anderen Projekten auf das freigegebene Projekt verwiesen wird, wird in der linken oberen Ecke des Quell Code-Editors ein Menü angezeigt, in dem Sie auswählen können, welche Projekte auf diese Datei verweisen.

## <a name="shared-project-options"></a>Optionen für freigegebene Projekte

Wenn Sie mit der rechten Maustaste auf ein frei gegebenes Projekt klicken und **Optionen** auswählen, sind weniger Einstellungen vorhanden als bei anderen Projekttypen. Da freigegebene Projekte nicht (eigenständig) kompiliert werden, können Sie keine Ausgabe-oder Compileroptionen, Projekt Konfigurationen, Assemblysignierung oder benutzerdefinierte Befehle festlegen. Der Code in einem freigegebenen Projekt erbt diese Werte effektiv von allen verweisen, die auf Sie verweisen.

Der Bildschirm **Optionen** wird unten angezeigt: der Projekt **Name** und der **Standard Namespace** sind die einzigen beiden Einstellungen, die Sie in der Regel ändern werden.

![](shared-projects-images/xs-sharedprojectoptions.png "Optionen für freigegebene Projekte")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="visual-studio-walkthrough"></a>Visual Studio: Exemplarische Vorgehensweise

In diesem Abschnitt wird erläutert, wie Sie ein frei gegebenes Projekt mithilfe von Visual Studio erstellen und verwenden. Eine komplette Implementierung finden Sie im Abschnitt zu frei [gegebenen Projektbeispielen](#Shared_Project_Example) .

### <a name="creating-a-shared-project"></a>Erstellen eines freigegebenen Projekts

Um ein neues frei gegebenes Projekt zu erstellen, navigieren Sie zu **Datei** > **Neues** > **Projekt**.

Geben Sie in Visual Studio 2019 im Suchfeld auf der Seite **Neues Projekt erstellen** den Wert frei **gegeben** ein. Wählen Sie die Vorlage für frei **gegebene Projekte** und dann **weiter**aus. Geben Sie einen Namen für das Projekt ein, und wählen Sie dann **Erstellen**aus.

Wählen Sie in Visual Studio 2017 die Vorlage für das frei **gegebene Projekt** aus, und wählen Sie dann einen Namen für das Projekt aus.

![Vorlage für freigegebene Projekte in Visual Studio 2017](shared-projects-images/vs-newsolution.png)

Sie können einer vorhandenen Projekt Mappe auch ein neues frei gegebenes Projekt hinzufügen, indem Sie mit der rechten Maustaste auf die Projektmappendatei klicken und **> Neues Projekt hinzufügen**auswählen Ein neues frei gegebenes Projekt sieht wie unten dargestellt aus (nachdem eine Klassendatei hinzugefügt wurde). Beachten Sie, dass es keine Verweise oder Komponenten Knoten gibt. Diese werden für freigegebene Projekte nicht unterstützt.

![](shared-projects-images/vs-empty.png "Leeres frei gegebenes Projekt")

Damit ein frei gegebenes Projekt nützlich ist, muss es von mindestens einem buildfähigen Projekt (z. b. einer IOS-oder Android-Anwendung oder-Bibliothek oder einem PCL-Projekt) referenziert werden. Ein frei gegebenes Projekt wird nicht kompiliert, wenn es nichts referenziert. Daher werden Syntax Fehler (oder andere) erst dann hervorgehoben, wenn von etwas anderem darauf verwiesen wird.

Das Hinzufügen eines Verweises auf ein frei gegebenes Projekt erfolgt auf die gleiche Weise wie das verweisen auf ein reguläres Bibliotheksprojekt. Dieser Screenshot zeigt ein xamarin. IOS-Projekt, das auf ein frei gegebenes Projekt verweist.

![](shared-projects-images/vs-reference.png "Projekt Verweis auf frei gegebenes Projekt")

Wenn von einer anderen Bibliothek oder Anwendung auf das freigegebene Projekt verwiesen wird, können Sie die Projekt Mappe erstellen und alle Fehler im Code anzeigen. Wenn von _zwei oder mehr_ anderen Projekten auf das freigegebene Projekt verwiesen wird, wird in der linken oberen Ecke des Quell Code-Editors ein Menü angezeigt, um zu sehen, welche Projekte auf die aktuelle Codedatei verweisen.

### <a name="shared-project-properties"></a>Freigegebene Projekteigenschaften

Wenn Sie ein frei gegebenes Projekt auswählen, sind im Eigenschaften Panel weniger Einstellungen als bei anderen Projekttypen vorhanden. Da freigegebene Projekte nicht (eigenständig) kompiliert werden, können Sie keine Ausgabe-oder Compileroptionen, Projekt Konfigurationen, Assemblysignierung oder benutzerdefinierte Befehle festlegen. Der Code in einem freigegebenen Projekt erbt diese Werte effektiv von allen verweisen, die auf Sie verweisen.

Der **Eigenschaften** Bereich wird unten angezeigt. der Stamm **Namespace** ist die einzige Einstellung, die Sie ändern können.

![](shared-projects-images/vs-sharedprojectproperties.png "Freigegebene Projekteigenschaften")

-----

<a name="Shared_Project_Example"/>

## <a name="shared-project-example"></a>Beispiel für einen gemeinsam genutzten

Das [Tasky](https://github.com/xamarin/mobile-samples/tree/master/Tasky) -Beispiel verwendet ein frei gegebenes Projekt, das sowohl von der IOS-, Android-als auch der Windows Phone-Anwendung verwendet wird. Sowohl die `SQLite.cs` - `TaskRepository.cs` als auch die-Quell Code Dateien nutzen Compilerdirektiven (z.b. `#if __ANDROID__`), um für jede der Anwendungen, die auf Sie verweisen, eine andere Ausgabe zu liefern.

Die gesamte Projektmappenstruktur ist unten dargestellt (in Visual Studio für Mac bzw. in Visual Studio):

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![](shared-projects-images/xs-examplesolution.png "Visual Studio für Mac Lösung")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](shared-projects-images/vs-examplesolution.png "Visual Studio-Projekt Mappe")

-----

Das Windows Phone Projekt kann innerhalb Visual Studio für Mac navigiert werden, auch wenn dieser Projekttyp nicht für die Kompilierung in Visual Studio für Mac unterstützt wird.

Die laufenden Anwendungen sind unten dargestellt:

![](shared-projects-images/example.png "IOS-, Android-Windows Phone Beispiele")

## <a name="summary"></a>Zusammenfassung

In diesem Dokument wurde beschrieben, wie freigegebene Projekte funktionieren, wie Sie in Visual Studio für Mac und Visual Studio erstellt und verwendet werden können, und es wurde eine einfache Beispielanwendung eingeführt, die ein frei gegebenes Projekt in Aktion veranschaulicht.

## <a name="related-links"></a>Verwandte Links

- [Tasky-Beispiel-App](https://github.com/xamarin/mobile-samples/tree/master/Tasky)
- [Portable Klassenbibliotheken (Beispiel)](~/cross-platform/app-fundamentals/pcl.md)
- [Freigabe von Code Optionen (Beispiel)](~/cross-platform/app-fundamentals/code-sharing.md)
