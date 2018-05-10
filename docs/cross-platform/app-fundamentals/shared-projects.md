---
title: Gemeinsam genutzte Projekte
description: Gemeinsam genutzte Projekte können Sie die gemeinsamen Code schreiben, der durch eine Reihe von verschiedenen Anwendungsprojekte verwiesen wird. Der Code wird als Teil jeder verweisenden Projekts kompiliert und zählen Compilerdirektiven Clientplattform-spezifische Funktionalität in der gemeinsamen Codebasis integrieren können.
ms.prod: xamarin
ms.assetid: 191c71fb-44a4-4e6c-af4b-7b1107dce6af
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 2910ada16aad2d8cb19cf0ee3a059c7df22b630e
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
---
# <a name="shared-projects"></a>Gemeinsam genutzte Projekte

_Gemeinsam genutzte Projekte können Sie die gemeinsamen Code schreiben, der durch eine Reihe von verschiedenen Anwendungsprojekte verwiesen wird. Der Code wird als Teil jeder verweisenden Projekts kompiliert und zählen Compilerdirektiven Clientplattform-spezifische Funktionalität in der gemeinsamen Codebasis integrieren können._

Shared-Projekte (manchmal auch als gemeinsam genutzte Ressource Projekte bezeichnet) können Sie Code schreiben, die mehrere Ziel-Projekte, einschließlich Xamarin Anwendungen gemeinsam verwendet werden.

Sie unterstützen Compilerdirektiven, sodass Sie bedingt einschließen, können plattformspezifischen Code in eine Teilmenge der Projekte kompiliert werden, die auf das freigegebene Projekt verweisen. Es gibt auch IDE-Unterstützung zum Verwalten der Compiler-Direktiven und visuell darzustellen, wie der Code in jeder Anwendung aussehen wird.

Wenn Sie in der Vergangenheit-Datei verknüpfen zur gemeinsamen Verwendung Code zwischen Projekten verwendet haben, funktioniert freigegebene Projekte auf ähnliche Weise aber mit deutlich verbesserter IDE-Unterstützung.



## <a name="what-is-a-shared-project"></a>Was ist ein freigegebenes Projekt?

Im Gegensatz zu den meisten anderen Projekttypen weisen ein freigegebenes Projekt keine Ausgabe (in Form von DLL), stattdessen wird der Code kompiliert, in jedes Projekt, die darauf verweist. Dies wird im folgenden Diagramm illustriert – vom Konzept her ist der gesamte Inhalt des freigegebenen Projekts "in den kopiert" jedes verweisenden Projekt und kompiliert, als wäre er ein Teil war.

 ![](shared-projects-images/sharedassetproject.png "Freigegebene Projektarchitektur")

Der Code in einem freigegebenen Projekt kann Compilerdirektiven enthalten, die aktivieren oder deaktivieren Codeabschnitte, die abhängig von der Anwendung Projekt den Code verwendet, das von den Feldern farbige Plattform im Diagramm vorgeschlagen wird.

Ein freigegebenes Projekt ist nicht auf einem eigenen kompiliert, es vorhanden ist, rein als eine Gruppierung von Quellcodedateien, die in anderen Projekten aufgenommen werden kann. Wenn von einem anderen Projekt verwiesen wird, wird der Code effektiv als kompiliert *Teil* dieses Projekts. Freigegebene Projekte können nicht auf jeden anderen Projekttyp (einschließlich von anderen Projekten gemeinsam genutzt) verweisen.

Beachten Sie, dass andere Projekte Android-Anwendung können nicht auf Android-Anwendungsprojekten verweisen – z. B. ein Android Komponententestprojekt kann nicht auf ein Android-Anwendungsprojekt verweisen. Weitere Informationen zu dieser Einschränkung finden Sie in diesem [Forumsdiskussion](http://forums.xamarin.com/discussion/comment/98092/).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)



## <a name="visual-studio-for-mac-walkthrough"></a>Exemplarische Vorgehensweise für Visual Studio für Mac


Dieser Abschnitt führt Sie durch die Schritte zum Erstellen und verwenden ein freigegebenes Projekt mit Visual Studio für Mac Finden Sie die zu [freigegebenen Projekt Beispiel](#Shared_Project_Example) Abschnitt, um ein vollständiges Beispiel.


## <a name="creating-a-shared-project"></a>Erstellen eines freigegebenen Projekts


Zum Erstellen eines neuen Projekts für Shared, navigieren Sie zu **Datei > Neues Projektmappen...**  und wählen Sie einen Namen.


![](shared-projects-images/xs-newsolution.png "Neue Projektmappe")


Sie können auch ein neues freigegebenes Projekt zu einer Projektmappe hinzufügen, indem Sie mit der rechten Maustaste auf die Projektmappendatei und **hinzufügen > Neues Projekt hinzufügen...** . Ein neues freigegebenes Projekt sieht wie folgt – Beachten Sie, dass keine Verweise oder Komponentenknoten; Diese werden für freigegebene Projekte nicht unterstützt.


![](shared-projects-images/xs-empty.png "Leere freigegebenen Projekts")


Für ein freigegebenes Projekt sind aussagekräftig sind muss er mindestens ein Build kann Projekt (z. B. eine iOS oder Android-Anwendung oder Bibliothek oder ein PCL-Projekt) verwiesen werden. Ein freigegebenes Projekt wird nicht kompiliert, wenn es nichts verweisen, dies der Fall ist die Syntax (oder andere) hat Fehler werden nicht hervorgehoben, bis er von einem anderen Element verwiesen wurde.



Hinzufügen eines Verweises auf ein freigegebenes Projekt erfolgt die gleiche Weise wie ein reguläres Steuerelementbibliothek-Projekt verweisen. Diese bildschirmabbildung zeigt ein Xamarin.iOS Projekt verweisen auf ein freigegebenes Projekt.


![](shared-projects-images/xs-reference.png "Freigegebene Projekt Projektverweis hinzu")


Sobald das freigegebene Projekt von einer anderen Bibliothek oder einer Anwendung verwiesen wird, können Sie erstellen Sie die Projektmappe und zeigen Sie alle Fehler im Code. Wenn das freigegebene Projekt verwiesen wird, durch _zwei oder mehr_ andere Projekte im angezeigten Menü in der linken oberen der Quellcode-Editor zeigt auswählen, welche Projekte diese Datei verweisen.



## <a name="shared-project-options"></a>Freigegebene Projektoptionen


Wenn Sie mit der rechten Maustaste auf ein freigegebenes Projekt, und wählen Sie **Optionen** es weniger Einstellungen als andere Projekttypen. Da freigegebene Projekte (eigenständig) nicht kompiliert werden, kann nicht Ausgabe- oder Compiler-Optionen, Projektkonfigurationen, Signieren von Assemblys oder benutzerdefinierten Befehlen festgelegt werden. Der Code in einem freigegebenen Projekt erbt effektiv, dass diese Werte nach Belieben einen auf verwiesen wird.



Die **Optionen** unterhalb - Projekt angezeigt wird **Namen** und die **Default Namespace** sind nur zwei Einstellungen, die Sie in der Regel geändert wird.


![](shared-projects-images/xs-sharedprojectoptions.png "Freigegebene Projektoptionen")



# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)



## <a name="visual-studio-walkthrough"></a>Visual Studio: Exemplarische Vorgehensweise


In diesem Abschnitt führt Sie durch die Schritte zum Erstellen und verwenden ein freigegebenes Projekt mit Visual Studio. Finden Sie die zu [freigegebenen Projekt Beispiel](#Shared_Project_Example) Abschnitt, um eine vollständige Implementierung.


### <a name="creating-a-shared-project"></a>Erstellen eines freigegebenen Projekts


Zum Erstellen eines neuen Projekts für Shared, navigieren Sie zu **Datei > Neues Projektmappen...**  , und wählen Sie einen Namen für das Projekt und Projektmappe.


![](shared-projects-images/vs-newsolution.png "Neue Projektmappe")


Sie können auch ein neues freigegebenes Projekt zu einer Projektmappe hinzufügen, indem Sie mit der rechten Maustaste auf die Projektmappendatei und **hinzufügen > Neues Projekt...** . Ein neues freigegebenes Projekt sieht wie unten dargestellt (nachdem Sie eine Klassendatei hinzugefügt wurde) – Beachten Sie, dass keine Verweise oder Komponentenknoten; Diese werden für freigegebene Projekte nicht unterstützt.


![](shared-projects-images/vs-empty.png "Leere freigegebenen Projekts")


Für ein freigegebenes Projekt sind aussagekräftig sind muss er mindestens ein Build kann Projekt (z. B. eine iOS oder Android-Anwendung oder Bibliothek oder ein PCL-Projekt) verwiesen werden. Ein freigegebenes Projekt wird nicht kompiliert, wenn es nichts verweisen, dies der Fall ist die Syntax (oder andere) hat Fehler werden nicht hervorgehoben, bis er von einem anderen Element verwiesen wurde.



Hinzufügen eines Verweises auf ein freigegebenes Projekt erfolgt die gleiche Weise wie ein reguläres Steuerelementbibliothek-Projekt verweisen. Diese bildschirmabbildung zeigt ein Xamarin.iOS Projekt verweisen auf ein freigegebenes Projekt.


![](shared-projects-images/vs-reference.png "Freigegebene Projekt Projektverweis hinzu")


Sobald das freigegebene Projekt von einer anderen Bibliothek oder einer Anwendung verwiesen wird, können Sie erstellen Sie die Projektmappe und zeigen Sie alle Fehler im Code. Wenn das freigegebene Projekt verwiesen wird, durch _zwei oder mehr_ andere Projekte, ein Menü wird angezeigt, in der oberen linken Ecke Source Code-Editor, um festzustellen, welche Projekte die aktuellen Codedatei verweisen.


### <a name="shared-project-properties"></a>Eigenschaften des freigegebenen Projekts


Wenn Sie ein freigegebenes Projekt gibt es weniger Einstellungen im Eigenschaftenbereich als andere Projekttypen auswählen. Da freigegebene Projekte (eigenständig) nicht kompiliert werden, kann nicht Ausgabe- oder Compiler-Optionen, Projektkonfigurationen, Signieren von Assemblys oder benutzerdefinierten Befehlen festgelegt werden. Der Code in einem freigegebenen Projekt erbt effektiv, dass diese Werte nach Belieben einen auf verwiesen wird.



Die **Eigenschaften** Bereich sehen Sie unten – die **Stamm-Namespace** ist die einzige Einstellung, die Sie ändern können.


![](shared-projects-images/vs-sharedprojectproperties.png "Eigenschaften des freigegebenen Projekts")



-----

<a name="Shared_Project_Example"/>

## <a name="shared-project-example"></a>Beispiel für freigegebenen Projekts

Die [Tasky](https://github.com/xamarin/mobile-samples/tree/master/Tasky) Beispiel von der beiden iOS, Android und Windows Phone-Anwendungen verwendeten verwendet ein freigegebenes Projekt den gemeinsamen Code enthalten. Sowohl die `SQLite.cs` und `TaskRepository.cs` Quellcodedateien verwenden die Compiler-Direktiven (z. b. `#if __ANDROID__`) an verschiedene Ausgaben für jede Anwendung zu erzeugen, die darauf verweisen.

Die vollständige Lösung-Struktur wird unten gezeigt (in Visual Studio für Mac und Visual Studio bzw.):

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

 ![](shared-projects-images/xs-examplesolution.png "Visual Studio für Mac-Lösung")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

 ![](shared-projects-images/vs-examplesolution.png "Visual Studio-Projektmappe")

-----

Das Windows Phone-Projekt kann von innerhalb von Visual Studio für Mac, navigiert werden, obwohl Sie diesen Projekttyp nicht unterstützt wird, für die Kompilierung in Visual Studio für Mac.

Nachfolgend finden Sie die Ausführung von Anwendungen.

 ![](shared-projects-images/example.png "iOS, Android, Windows Phone-Beispiele")



## <a name="summary"></a>Zusammenfassung

Dieses Dokument beschrieben, wie freigegebene Projekte funktionieren, wie sie erstellt und verwendet werden können in Visual Studio für Mac und Visual Studio, und eine einfache beispielanwendung, die ein freigegebenes Projekt in Aktion veranschaulicht eingeführt.

## <a name="related-links"></a>Verwandte Links

- [Tasky Beispiel-App](https://github.com/xamarin/mobile-samples/tree/master/Tasky)
- [Portable Klassenbibliotheken (Beispiel)](~/cross-platform/app-fundamentals/pcl.md)
- [Optionen (Beispiel) für die Codefreigabe](~/cross-platform/app-fundamentals/code-sharing.md)
