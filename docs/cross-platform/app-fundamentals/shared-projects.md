---
title: Verwenden von freigegebenen Projekten zum Freigeben von Code
description: Freigegebene Projekte können Sie die gemeinsamen Code zu schreiben, der durch eine Reihe von verschiedenen Anwendungsprojekte verwiesen wird. Der Code wird als Teil jeder verweisende Projekt kompiliert und zählen Compiler-Direktiven können plattformspezifische Funktionalität in die gemeinsame Codebasis zu integrieren.
ms.prod: xamarin
ms.assetid: 191c71fb-44a4-4e6c-af4b-7b1107dce6af
author: conceptdev
ms.author: crdun
ms.date: 07/18/2018
ms.openlocfilehash: 61096635cd94d0fdd0abe6fda59c4efa41eeceb1
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61193226"
---
# <a name="shared-projects-code-sharing"></a>Freigegebene Projekte freigeben von code

_Freigegebene Projekte können Sie die gemeinsamen Code zu schreiben, der durch eine Reihe von verschiedenen Anwendungsprojekte verwiesen wird. Der Code wird als Teil jeder verweisende Projekt kompiliert und zählen Compiler-Direktiven können plattformspezifische Funktionalität in die gemeinsame Codebasis zu integrieren._

Freigegebene Projekte (manchmal auch als "freigegebene Asset-Projekte" bezeichnet) können Sie Code schreiben, der mehrere Zielprojekte, einschließlich Xamarin-Anwendungen gemeinsam verwendet wird.

Sie unterstützt die Compiler-Direktiven, sodass Sie bedingt einschließen, können plattformspezifischen Code in eine Teilmenge der Projekte kompiliert werden, die das freigegebene Projekt verweisen. Es gibt auch IDE-Unterstützung zum Verwalten der Compiler-Direktiven und visualisieren, wie der Code in jeder Anwendung aussehen wird.

Wenn Sie in der Vergangenheit dateiverknüpfungen zum Freigeben von Code zwischen Projekten verwendet haben, funktioniert freigegebene Projekte auf ähnliche Weise, aber mit viel bessere IDE-Unterstützung.

## <a name="what-is-a-shared-project"></a>Was ist ein freigegebenes Projekt?

Im Gegensatz zu den meisten anderen Projekttypen ein freigegebenes Projekt verfügt nicht über keine Ausgabe (in Form der DLL), stattdessen wird der Code kompiliert, in jedem Projekt, die darauf verweist. Dies wird im folgenden Diagramm veranschaulicht: der gesamte Inhalt des freigegebenen Projekts vom Konzept her ist "in kopiert" jedes Projekt verweisen und kompiliert, als wäre es ein Teil war.

![](shared-projects-images/sharedassetproject.png "Freigegebenen Projekt-Architektur")

Der Code in ein freigegebenes Projekt kann es sich um Compiler-Direktiven enthalten, die aktivieren oder Deaktivieren von Codeabschnitten, abhängig von der Anwendung Projekt Code verwendet wird, die durch die farbige Plattform Felder im Diagramm wird empfohlen.

Ein freigegebenes Projekt ist nicht allein kompiliert, es vorhanden ist, rein als eine Gruppierung von Quellcodedateien, die in anderen Projekten enthalten sein können. Wenn von einem anderen Projekt verwiesen wird, wird der Code effektiv als kompiliert *Teil* dieses Projekts. Freigegebene Projekte können nicht auf einem anderen Projekttyp (einschließlich andere Projekte freigegeben) verweisen.

Beachten Sie, dass Android-Anwendungsprojekten können nicht auf andere Android-Anwendungsprojekten verweisen – z. B. ein Android Komponententestprojekt kann nicht auf einem Android-Anwendungsprojekt verweisen. Weitere Informationen zu dieser Einschränkung finden Sie in diesem [Forumsdiskussion](http://forums.xamarin.com/discussion/comment/98092/).

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

## <a name="visual-studio-for-mac-walkthrough"></a>Exemplarische Vorgehensweise für Visual Studio für Mac

In diesem Abschnitt führt Sie durch die Schritte zum Erstellen und verwenden ein freigegebenes Projekt mit Visual Studio für Mac. Finden Sie die zu [freigegebenen Projekt Beispiel](#Shared_Project_Example) Abschnitt, um ein vollständiges Beispiel.

## <a name="creating-a-shared-project"></a>Erstellen eines freigegebenen Projekts

Zum Erstellen ein neues freigegebenes Projekt, navigieren Sie zu **Datei > neue Projektmappe...**  (oder klicken Sie mit der rechten Maustaste auf eine vorhandene Projektmappe und Auswahl **hinzufügen > Neues Projekt hinzufügen...** ):

[![Neues freigegebenes Projekt](shared-projects-images/xs-newsolution-sml.png "neue Projektmappe")](shared-projects-images/xs-newsolution.png#lightbox)

Klicken Sie auf dem nächsten Bildschirm Wählen Sie den Namen des Projekts, und klicken Sie auf **erstellen**.

Ein neues freigegebenes Projekt wird im folgenden dargestellt: Es sind keine Verweise oder Komponentenknoten; Diese werden für freigegebene Projekte nicht unterstützt.

![Leere freigegebenes Projekt](shared-projects-images/xs-empty.png "leere freigegebenes Projekt")

Für ein freigegebenes Projekt nützlich ist muss sie mindestens einen Build kann Projekt (z. B. eine iOS oder Android-Anwendung oder Bibliothek bzw. ein PCL-Projekt) verwiesen werden. Ein freigegebenes Projekt wird nicht kompiliert, wenn es nichts verweisen, dies der Fall ist die Syntax (oder andere) hat Fehler werden nicht hervorgehoben werden, bis er von einem anderen Element verwiesen wurde.

Hinzufügen eines Verweises auf ein freigegebenes Projekt erfolgt die gleiche Weise wie die verweisende eines regulären Library-Projekts. Dieser Screenshot zeigt ein Xamarin.iOS-Projekt verweisen auf ein freigegebenes Projekt.

![](shared-projects-images/xs-reference.png "Projektverweis auf freigegebenes Projekt")

Sobald das freigegebene Projekt von einer anderen Bibliothek oder einer Anwendung verwiesen wird, können Sie erstellen Sie die Projektmappe und zeigen Sie alle Fehler im Code. Wenn das freigegebene Projekt verwiesen wird, indem _zwei oder mehr_ anderen Projekten ein Menü wird angezeigt in der oberen linken Ecke des der Quellcode-Editor zeigt auswählen, welche Projekte diese Datei verweisen.

## <a name="shared-project-options"></a>Shared-Projektoptionen

Wenn Sie mit der rechten Maustaste auf ein freigegebenes Projekt, und wählen Sie **Optionen** gibt es weniger Einstellungen als andere Projekttypen. Da freigegebene Projekte (selbstständig) nicht kompiliert werden, können keine Ausgabe- oder Compiler-Optionen, Projektkonfigurationen, Signieren von Assemblys oder benutzerdefinierten Befehlen festgelegt werden. Der Code in ein freigegebenes Projekt erbt effektiv, dass diese Werte was auch immer sie verweist.



Die **Optionen** unterhalb - Projekt angezeigt wird **Name** und **Default Namespace** sind die nur zwei Einstellungen, die Sie in der Regel geändert wird.


![](shared-projects-images/xs-sharedprojectoptions.png "Shared-Projektoptionen")



# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)



## <a name="visual-studio-walkthrough"></a>Visual Studio: Exemplarische Vorgehensweise


In diesem Abschnitt erläutert, wie zum Erstellen und verwenden ein freigegebenes Projekt mithilfe von Visual Studio. Finden Sie die zu [freigegebenen Projekt Beispiel](#Shared_Project_Example) Abschnitt, um eine vollständige Implementierung.

### <a name="creating-a-shared-project"></a>Erstellen eines freigegebenen Projekts

Zum Erstellen ein neues freigegebenes Projekt, navigieren Sie zu **Datei > neue Projektmappe...**  , und wählen Sie einen Namen für das Projekt und Projektmappe.

![](shared-projects-images/vs-newsolution.png "Neue Projektmappe")

Sie können auch ein neues freigegebenes Projekt zu einer Projektmappe hinzufügen, indem mit der rechten Maustaste auf die Projektmappendatei und **hinzufügen > Neues Projekt...** . Ein neues freigegebenes Projekt sieht aus wie unten dargestellt (nachdem eine Datei hinzugefügt wurde): Beachten Sie, dass keine Verweise vorhanden sind oder Komponentenknoten; Diese werden für freigegebene Projekte nicht unterstützt.

![](shared-projects-images/vs-empty.png "Leere freigegebenes Projekt")

Für ein freigegebenes Projekt nützlich ist muss sie mindestens einen Build kann Projekt (z. B. eine iOS oder Android-Anwendung oder Bibliothek bzw. ein PCL-Projekt) verwiesen werden. Ein freigegebenes Projekt wird nicht kompiliert, wenn es nichts verweisen, dies der Fall ist die Syntax (oder andere) hat Fehler werden nicht hervorgehoben werden, bis er von einem anderen Element verwiesen wurde.

Hinzufügen eines Verweises auf ein freigegebenes Projekt erfolgt die gleiche Weise wie die verweisende eines regulären Library-Projekts. Dieser Screenshot zeigt ein Xamarin.iOS-Projekt verweisen auf ein freigegebenes Projekt.

![](shared-projects-images/vs-reference.png "Projektverweis auf freigegebenes Projekt")

Sobald das freigegebene Projekt von einer anderen Bibliothek oder einer Anwendung verwiesen wird, können Sie erstellen Sie die Projektmappe und zeigen Sie alle Fehler im Code. Wenn das freigegebene Projekt verwiesen wird, indem _zwei oder mehr_ anderen Projekten ein Menü wird angezeigt, in der linken oberen Ecke im Source Code-Editor, um festzustellen, welche Projekte die aktuellen Codedatei verweisen.


### <a name="shared-project-properties"></a>Eigenschaften des freigegebenen Projekts


Wenn Sie ein freigegebenes Projekt gibt es weniger Einstellungen im Bereich "Eigenschaften" als andere Projekttypen auswählen. Da freigegebene Projekte (selbstständig) nicht kompiliert werden, können keine Ausgabe- oder Compiler-Optionen, Projektkonfigurationen, Signieren von Assemblys oder benutzerdefinierten Befehlen festgelegt werden. Der Code in ein freigegebenes Projekt erbt effektiv, dass diese Werte was auch immer sie verweist.

Die **Eigenschaften** Bereich wird unten – die **Stamm-Namespace** ist die einzige Einstellung, die Sie ändern können.

![](shared-projects-images/vs-sharedprojectproperties.png "Eigenschaften des freigegebenen Projekts")

-----

<a name="Shared_Project_Example"/>

## <a name="shared-project-example"></a>Beispiel für freigegebenen Projekt

Die [Tasky](https://github.com/xamarin/mobile-samples/tree/master/Tasky) Beispiel verwendet ein freigegebenes Projekt enthält den allgemeinen Code, der von der sowohl iOS, Android und Windows Phone-Anwendungen verwendet. Sowohl die `SQLite.cs` und `TaskRepository.cs` Quellcodedateien Nutzung compileranweisungen (z. b. `#if __ANDROID__`), die andere Ausgabe für jede der Anwendungen zu erstellen, die darauf verweisen.

Die Struktur der vollständigen Lösung wird unten gezeigt (in Visual Studio für Mac und Visual Studio entsprechend):

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![](shared-projects-images/xs-examplesolution.png "Visual Studio für Mac-Lösung")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](shared-projects-images/vs-examplesolution.png "Visual Studio-Projektmappe")

-----

Das Windows Phone-Projekt kann innerhalb von Visual Studio für Mac, gewechselt werden, obwohl Sie diesen Projekttyp nicht unterstützt wird, für die Kompilierung in Visual Studio für Mac.

Die ausgeführten Anwendungen werden unten angezeigt:

![](shared-projects-images/example.png "iOS, Android, Windows Phone-Beispielen")

## <a name="summary"></a>Zusammenfassung

In diesem Dokument beschrieben, wie freigegebene Projekte zu arbeiten, wie sie erstellt und in Visual Studio für Mac und Visual Studio verwendet werden, und eine einfache beispielanwendung, die ein freigegebenes Projekt in Aktion zeigt eingeführt.

## <a name="related-links"></a>Verwandte Links

- [Tasky Beispiel-App](https://github.com/xamarin/mobile-samples/tree/master/Tasky)
- [Portable Klassenbibliotheken (Beispiel)](~/cross-platform/app-fundamentals/pcl.md)
- [Optionen (Beispiel) für die Codefreigabe](~/cross-platform/app-fundamentals/code-sharing.md)
